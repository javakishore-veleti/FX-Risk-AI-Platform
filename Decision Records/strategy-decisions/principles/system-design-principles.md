# System Design Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Biannually or after major system architecture changes or service refactors

## Audience

This document is intended for:

- Backend and Platform Engineers  
- Architects and Tech Leads  
- DevOps and SRE Teams  
- Engineering Managers responsible for system delivery

## Governance

- System designs must be reviewed for compliance with these principles before implementation  
- Violations must be justified with tradeoffs and logged in ADRs  
- Architecture review boards use this document as part of scoring and approval criteria

## Automation and Tooling Enablers

- **Architecture Review Checklists**: Mapped to these principles and enforced during design reviews  
- **Resilience Testing Tools**: Chaos testing, load simulation, latency injection  
- **CI/CD Gatekeeping**: Prevent misaligned architectural choices (e.g., anti-patterns, missing health checks)  
- **Infrastructure Templates**: Terraform/CDK scaffolds aligned with modular service design

---

## Principle Set

### 1. Single Responsibility and Domain Isolation

- Each service must focus on one domain or business capability  
- Cross-cutting concerns (auth, logging, orchestration) should be externalized and reused

### 2. Fail-Fast and Fail-Visible

- Components should validate inputs early and fail clearly  
- Errors must be logged with traceability; downstream systems must not silently absorb failures

### 3. Idempotency and Retry Logic

- All state-changing operations must be idempotent  
- Asynchronous workflows must handle retries, DLQs, and deduplication

### 4. Loose Coupling and Clear Contracts

- Prefer contracts over shared databases or runtime coupling  
- Services should evolve independently with versioned APIs or event formats

### 5. Scalability and Elasticity

- Design for horizontal scaling with stateless services  
- Storage, queues, and compute must autoscale based on demand and usage profiles

### 6. Resilience and Graceful Degradation

- Systems must detect and recover from dependency failure (timeouts, fallbacks, circuit breakers)  
- Core business flows must remain operational during partial outages

### 7. Observability by Default

- All systems must emit logs, metrics, and traces using platform-standard formats  
- Golden signals (latency, error rate, throughput, saturation) must be tracked per service

### 8. Secure by Design

- Apply least privilege IAM, encrypted transport/storage, and input validation  
- Use shared libraries for auth and secrets handling

### 9. Modular and Composable

- Encourage reuse of scaffolds, job runners, logging wrappers, and tracing libraries  
- Favor composition over inheritance or tight class hierarchies

### 10. Deployment and Operational Readiness

- Each service must provide `/health` and `/ready` endpoints  
- Blue/green or canary deployments must be supported for major changes  
- On-call teams must have dashboards, alerts, and runbooks for each system
