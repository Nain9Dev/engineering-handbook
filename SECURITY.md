# Security & Vulnerability Disclosure Policy

## Reporting a Vulnerability

Security and client data confidentiality are fundamental tenets of the NainDev ecosystem. If you discover a security vulnerability or accidental information exposure, please report it privately:

- **Email**: [security@naindev.com](mailto:security@naindev.com)
- **Direct Contact**: Aitor Nain Mendoza Vallejo

Please do not disclose security issues or vulnerabilities publicly via GitHub issues or discussions until they have been reviewed and remediated.

---

## Confidentiality & Isolation Architecture

1. **Commercial Isolation**: Client engagements operate inside isolated private boundaries. No confidential client business logic, internal API topologies, or private schemas are mirrored to public repositories.
2. **Automated Secret Screening**: All repositories maintain continuous automated secret detection gates to ensure credentials, tokens, and private hostnames are blocked prior to commit.
3. **Deterministic Sanitization**: Public artifacts derived from architectural audits undergo deterministic sanitization against banned regex tokens.
