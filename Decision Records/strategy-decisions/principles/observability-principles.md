# Observability Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Quarterly or upon observability platform upgrades or SLA breaches

## Audience

This document is intended for:

- Platform Engineers and SREs  
- Application Developers and Tech Leads  
- DevOps and Monitoring Teams  
- QA and Incident Response Analysts

## Governance

- All services must meet baseline observability standards before production deployment  
- SLAs and SLOs must be backed by measurable metrics and alerting rules  
- Observability is a mandatory part of architecture and design reviews

## Automation and Tooling Enablers

- **Tracing**: OpenTelemetry or AWS X-Ray enabled on all service calls  
- **Metrics**: CloudWatch, Prometheus, or embedded metrics for latency, error rates, and saturation  
- **Logging**: Structured logs with correlation IDs via shared logger wrappers  
- **Dashboards**: Automated generation of service-level views per repo  
- **Alerting**: Defined thresholds, ownership, and escalation paths

---

## Principle Set

### 1. Measure What Matters

- Capture service-level indicators (SLIs) such as success rate, latency, and throughput  
- Define SLOs (Service Level Objectives) with stakeholders (e.g., 99.9% success in 1s)  
- Map SLIs to business impact (e.g., forecast availability, audit latency)

### 2. Trace Every Request

- Each incoming request must carry a unique `traceId` and `requestId`  
- Spans must be propagated across microservices and external calls  
- Traces must show parent-child relationship and time deltas

### 3. Log with Structure and Context

- Use JSON logs with fields: `timestamp`, `level`, `message`, `traceId`, `userId`, `deskId`, `service`, `action`  
- Never log secrets or raw PII  
- Logs must be queryable and retained according to compliance policy

### 4. Alert Before It Hurts

- Alerts must trigger for sustained breaches of SLOs (not on noise or single failure)  
- Alerts must include metadata (service, region, traceId) and be routed to an owning team  
- Alerts should auto-resolve and be tracked for MTTR and RCA

### 5. Standardized Metrics

- Emit metrics in a common namespace (e.g., `fxai.forecasting.latency`, `fxai.audit.error_rate`)  
- Include tags for `env`, `region`, `desk`, and `component`  
- Share standard templates for HTTP, DB, and queue integrations

### 6. Health and Readiness Probes

- Each service must expose `/health` and `/ready` endpoints  
- Probes must reflect real backend connectivity (e.g., DB, queues, external APIs)

### 7. Dashboards for Everyone

- Dashboards must be provided per team/service with key KPIs  
- Ops and business users must understand service state without parsing logs  
- Dashboards must include golden signals (latency, errors, saturation, traffic)

### 8. Retention and Compliance

- Logs must be retained per audit policy (e.g., 90 days minimum)  
- Sensitive fields must be masked or redacted  
- Access to logs and dashboards must follow RBAC and audit trail enforcement

### 9. Performance Budgeting

- Services must monitor and budget for CPU, memory, cold start time, and DB latency  
- Excessive resource usage must trigger alerts and remediation

### 10. Observability by Design

- Observability must be built into the first commit, not bolted on  
- Scaffolds must include logging, metrics, and tracing decorators by default  
- Every feature, bug, or refactor must consider its impact on visibility
