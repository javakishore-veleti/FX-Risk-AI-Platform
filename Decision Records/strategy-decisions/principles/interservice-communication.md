# Interservice Communication Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Annually or after introduction of new services, protocols, or orchestration patterns

## Audience

This document is intended for:

- Application Developers  
- API Designers and Platform Engineers  
- Architects designing microservices or event-driven systems  
- Security and Observability Teams

## Governance

- All interservice communications must follow approved protocols and observability requirements  
- Architecture reviews must document the communication method, retry behavior, and error propagation strategy  
- All exposed endpoints must be secured, discoverable, and versioned

## Automation and Tooling Enablers

- **Service Registry**: Shared contract registry for service names, owners, APIs, and topics  
- **Async Retry Decorators**: Shared Python/Node libraries for retries and exponential backoff  
- **Auth Middlewares**: Common token validation and permission enforcement modules  
- **Tracing Hooks**: Automatic `traceId` injection for logs, metrics, and spans

---

## Principle Set

### 1. Explicit Interface Contracts

- All services must publish request/response schemas or event payload formats  
- Use OpenAPI for HTTP APIs and Avro/JSON Schema for messaging patterns  
- Version all contracts explicitly (`/v1/...`, `topic.name.v1`)

### 2. API-First over Internal DB Access

- Services must never read or write each other’s databases  
- Use APIs or message queues to request or react to data and state changes

### 3. Sync for Read, Async for Workflow

- Use REST/GraphQL for real-time queries or lookups  
- Use SQS, SNS, or EventBridge for event-driven orchestration, side effects, or fanout

### 4. Secure by Default

- All APIs must require IAM/JWT/role tokens; no anonymous endpoints  
- Token must include userId, deskId, traceId, and be validated by middleware  
- Queue access must use IAM roles, not hardcoded credentials

### 5. Reliable and Idempotent

- All idempotent endpoints must accept `Idempotency-Key` headers  
- Async messages must support retry detection (e.g., message ID or hash)

### 6. Backpressure and Rate Controls

- Async consumers must be throttled with DLQs and retry delays  
- APIs must respond with `429 Too Many Requests` and support client retries with backoff

### 7. Observability Across Boundaries

- All messages and HTTP calls must include `traceId`, `spanId`, and `requestId`  
- Use AWS X-Ray or OpenTelemetry to stitch traces across services

### 8. Graceful Degradation

- Callers must handle timeouts, 5xx errors, or failed callbacks gracefully  
- Fallback logic or circuit breakers must be implemented for critical dependencies

### 9. Evolution Without Breakage

- New fields in messages must be backward compatible  
- Retire old versions only after consumers have migrated and confirmed

### 10. Discoverability and Ownership

- Each service must register its API endpoints or event topics in the service catalog  
- Include team owner, escalation contact, and SLA for response or delivery
