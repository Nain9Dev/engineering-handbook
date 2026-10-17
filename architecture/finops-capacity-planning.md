# FinOps Architecture & Production Capacity Planning

> Two-tier cost modeling, sizing calculations, and infrastructure economics: from 0,00 EUR portfolio demonstration to 10,000 req/min enterprise production.

---

## 1. Executive Summary & Economic Philosophy

A senior software architect must demonstrate financial stewardship alongside technical excellence. The NainDev platform uses a **Two-Tier FinOps Architecture**:
1. **Tier 0 (Demonstration & Portfolio Tier)**: Operates at exactly **0,00 EUR** recurrent monthly cost, proving that modern cloud-native architectures can achieve high availability, fast response times, and full functional completeness without burn rate.
2. **Tier 1 (Enterprise Production Tier)**: A fully modeled, right-sized cloud infrastructure plan capable of sustaining **10,000 requests/minute** (~14,400,000 requests/day) under strict Service Level Agreements (99.95% availability, RPO < 5 minutes, RTO < 15 minutes).

---

## 2. Tier 0: Zero-Cost Architectural Baseline (0,00 EUR/month)

The demonstration tier leverages static compilation, edge computing, serverless databases, and public automation:

| Layer | Service / Provider | Tier / SKU | Monthly Expense | Architecture Role |
| :--- | :--- | :--- | :--- | :--- |
| **Edge / CDN / DNS** | Cloudflare | Free Tier | 0,00 EUR | Global Anycast DNS, SSL termination, DDoS protection, edge caching. |
| **Static Frontends** | GitHub Pages | Free Public Repo Tier | 0,00 EUR | Serves client SPAs (`brand`, `status`, client panels) from multi-region CDN. |
| **Compute & APIs** | Self-Hosted / Local Docker | Local Container Runtime | 0,00 EUR | Full Docker Compose orchestration for all .NET, Python, and RabbitMQ services. |
| **Observability** | GitHub Actions | 2,000 free runner mins / public | 0,00 EUR | Automated blackbox probing every 30m, generating static dashboard artifacts. |
| **Artifact Storage** | GitHub Packages / Releases | Free Tier | 0,00 EUR | Container images and build artifact distribution. |
| **Total Tier 0 Expense** | | | **0,00 EUR / month** | |

---

## 3. Tier 1: Enterprise Production Capacity Model (10,000 req/min)

### 3.1 Workload Profile & Sizing Assumptions
- **Target Throughput**: 10,000 requests/minute (166.7 req/sec average, 500 req/sec peak burst).
- **Daily Request Volume**: ~14.4 million requests/day.
- **Payload Distribution**:
  - 85% Read queries (sub-millisecond via Dapper and Redis cache).
  - 15% Transactional write commands (EF Core Unit of Work with optimistic concurrency).
- **Network Egress**: ~1.2 TB / month.

### 3.2 Projected Cloud Infrastructure Bill (Azure Enterprise Baseline)

| Component | Architecture Role | Sizing / SKU | Redundancy | Estimated Monthly Cost |
| :--- | :--- | :--- | :--- | :--- |
| **Container Compute** | Azure Container Apps (ACA) | 2 vCPU, 4 GB RAM (Autoscale: 2 to 8 replicas) | Multi-Zone | ~58,00 EUR |
| **Database (PostgreSQL)** | Azure Database for PostgreSQL Flexible | General Purpose D2ds_v5 (2 vCPU, 8 GB RAM, 128 GB Premium SSD, 3,000 IOPS) | Zone-Redundant HA with automatic failover | ~112,00 EUR |
| **Cache & Distributed Locks** | Azure Cache for Redis | Standard C1 (1 GB RAM, Dual-node master/replica) | Multi-AZ replication | ~48,00 EUR |
| **Message Broker** | Azure Service Bus (or Managed RabbitMQ) | Standard Tier (Base unit + 15M operations) | Built-in geo-replication | ~22,00 EUR |
| **Edge & WAF** | Cloudflare Pro / Azure Front Door | Pro Tier with Managed WAF rules | Global Anycast | ~20,00 EUR |
| **Observability & Logs** | Azure Monitor / Application Insights | 5 GB ingestion / month + 30-day retention | Centralized log workspace | ~15,00 EUR |
| **Total Tier 1 Expense** | | | | **~275,00 EUR / month** |

### 3.3 Alternative Cost-Optimized VPS Blueprint (Hetzner Dedicated Infrastructure)

For enterprises seeking sovereign, cost-optimized European infrastructure without hyperscaler lock-in:

| Component | Sizing / Node | Monthly Cost |
| :--- | :--- | :--- |
| **Primary Compute Node** | Hetzner Cloud CPX31 (4 Dedicated vCPU, 8 GB RAM, 160 GB NVMe) | ~14,50 EUR |
| **Secondary Replica Node** | Hetzner Cloud CPX31 (4 Dedicated vCPU, 8 GB RAM, 160 GB NVMe) | ~14,50 EUR |
| **Managed PostgreSQL** | Managed PostgreSQL HA Cluster (2 Nodes, Automated Backups) | ~39,00 EUR |
| **Managed RabbitMQ** | CloudAMQP Lemur / Bunny Dedicated Instance | ~19,00 EUR |
| **Backups & Cold Storage** | Hetzner Storage Box (1 TB, encrypted rsync/Borg) | ~3,80 EUR |
| **Total Optimized VPS Expense** | | **~90,80 EUR / month** |

---

## 4. Disaster Recovery & Business Continuity (BC/DR)

- **Recovery Point Objective (RPO)**: **< 5 minutes**. Continuous write-ahead logging (WAL) archiving to secondary geo-replicated object storage.
- **Recovery Time Objective (RTO)**: **< 15 minutes**. Automated health check probing triggers DNS failover via Cloudflare API. Infrastructure provisioned as code (IaC) via Docker Compose and Terraform recipes.
- **Data Protection at Rest**: AES-256 bit encryption across all database tablespaces and WAL archives.

---

## 5. Cost Governance & Anomaly Detection Guardrails

1. **Budget Alert Thresholds**: Automated webhooks fire at 50%, 75%, and 90% of monthly budget allocations.
2. **Autoscaling Circuit Breakers**: Max replica ceilings prevent runaway autoscaling during distributed denial-of-service (DDoS) events.
3. **Automated Ephemeral Teardown**: Non-production staging environments automatically shut down outside business hours (saving ~65% on staging compute).
