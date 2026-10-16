# Dual Persistence Pattern (EF Core + Dapper)

> Standardized CQRS-style persistence architecture combining transactional aggregate integrity with high-throughput zero-allocation read projections.

---

## 1. Problem Statement

Object-Relational Mappers (ORMs) such as EF Core excel at managing complex entity graphs, change tracking, and state invariant enforcement during writes. However, using ORMs for extensive reporting, analytical dashboards, and deeply nested read projections introduces severe drawbacks:

1. **Tracking Overhead**: Entity graph allocation and state tracking consume memory and CPU cycles.
2. **Suboptimal SQL Generation**: Auto-generated joins across multiple navigation properties result in cartesian explosions or N+1 queries.
3. **Impedance Mismatch**: Forcing read queries through domain entities artificially restricts query freedom and pollutes domain classes with read-only concerns.

Conversely, micro-ORMs like Dapper deliver raw ADO.NET execution speed and full control over SQL, but lack change tracking and automated migration capabilities needed for complex domain aggregates.

---

## 2. The Architectural Solution

NainDev microservices utilize **Dual Persistence**:

- **Command Pipeline (Writes)**: Exclusively routed through **Entity Framework Core**.
- **Query Pipeline (Reads)**: Exclusively routed through **Dapper** with raw, indexed SQL queries.

```
       [Client Request]
              |
              +---------------------+
              |                     |
       (Command / Mutation)   (Query / Read)
              |                     |
              v                     v
       Application Layer     Application Layer
         Command Handler       Query Handler
              |                     |
              v                     v
           EF Core               Dapper
         (Unit of Work)    (Raw SQL Projection)
              |                     |
              +----------+----------+
                         |
                         v
                [PostgreSQL Database]
```

---

## 3. Rules of Engagement

### 3.1 Write Side (EF Core)
1. **Aggregates as Boundaries**: Repositories only load and save aggregate roots. Child entities are manipulated through methods on the root entity.
2. **Strict Invariant Protection**: Domain entities expose encapsulated mutation methods. Public property setters (`set;`) are prohibited.
3. **Change Tracking Enabled**: Tracking is retained during write operations to detect mutations and trigger concurrency checks.
4. **Optimistic Concurrency**: Aggregates carry a concurrency token (`RowVersion` or `xmin` in PostgreSQL) to prevent lost updates without database row locks.

### 3.2 Read Side (Dapper)
1. **Domain Model Bypass**: Query handlers never instantiate domain entities. They project SQL query results directly into lightweight flat Read Models / DTOs.
2. **Single Trip Queries**: Use PostgreSQL `json_build_object` or Dapper multi-mapping to retrieve complex hierarchical views in a single database round-trip.
3. **No Dynamic SQL String Interpolation**: All queries use parameterized SQL (`@ParameterName`) to prevent SQL injection and allow query plan caching.
4. **Read-Only Connections**: Where supported, queries target read-replica connection strings or explicit `READ ONLY` database transactions.

---

## 4. Code Sample (.NET 10)

```csharp
// --- WRITE SIDE: EF Core Unit of Work ---
public class ConfirmOrderHandler : IRequestHandler<ConfirmOrderCommand, Result>
{
    private readonly NainOrderDbContext _context;

    public ConfirmOrderHandler(NainOrderDbContext context) => _context = context;

    public async Task<Result> Handle(ConfirmOrderCommand cmd, CancellationToken ct)
    {
        var order = await _context.Orders
            .Include(o => o.LineItems)
            .FirstOrDefaultAsync(o => o.Id == cmd.OrderId, ct);

        if (order is null) return Result.NotFound();

        order.Confirm(cmd.ConfirmedBy); // Enforces state invariant
        await _context.SaveChangesAsync(ct);
        return Result.Success();
    }
}

// --- READ SIDE: Dapper Projection ---
public class GetOrderDashboardQueryHandler : IRequestHandler<GetOrderDashboardQuery, OrderDashboardDto>
{
    private readonly IDbConnectionFactory _connectionFactory;

    public GetOrderDashboardQueryHandler(IDbConnectionFactory factory) => _connectionFactory = factory;

    public async Task<OrderDashboardDto> Handle(GetOrderDashboardQuery query, CancellationToken ct)
    {
        const string sql = """
            SELECT 
                o.id AS OrderId,
                o.status AS Status,
                o.total_amount AS TotalAmount,
                o.created_at AS CreatedAt,
                COUNT(i.id) AS ItemCount
            FROM orders o
            LEFT JOIN order_items i ON o.id = i.order_id
            WHERE o.customer_id = @CustomerId
            GROUP BY o.id, o.status, o.total_amount, o.created_at
            ORDER BY o.created_at DESC
            LIMIT 50;
            """;

        using var connection = _connectionFactory.CreateConnection();
        var results = await connection.QueryAsync<OrderDashboardDto>(
            new CommandDefinition(sql, new { query.CustomerId }, cancellationToken: ct));

        return results;
    }
}
```
