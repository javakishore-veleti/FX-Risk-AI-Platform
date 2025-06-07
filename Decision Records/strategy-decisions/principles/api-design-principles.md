# API Design Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform
- **Review Cadence**: Annual or after significant changes to API gateway or orchestration layers

## Audience

This document is intended for the following stakeholders:

- Backend Engineers
- API Gateway Designers
- Platform Engineers
- Consumer Teams Integrating with Platform APIs
- Observability and Security Engineers

## Governance

- All new APIs must adhere to these principles before review or merge
- Changes to shared APIs (used across desks or modules) require architecture review sign-off
- Violations should be flagged in pull requests via automated linters and manual review

## Automation and Tooling Enablers

- **OpenAPI Spec Enforcement**: Required for all service APIs, validated during CI
- **Versioning**: Semantic versioning for endpoints, changes require changelog and impact review
- **Auth & Scope Tags**: Every endpoint must declare expected JWT claims and role scopes
- **Rate Limiting**: All public/external-facing APIs must enforce AWS API Gateway throttling policies
- **Linter Checks**: Use Spectral or custom linters to validate naming, errors, and schema reuse
- **Observability Hooks**: Standardized logging middleware for trace ID, user ID, and latency

---

## Principle Set

The following principles define the baseline for designing and maintaining APIs across the platform:

### 1. Contract-First Design

- Define OpenAPI or GraphQL schema before implementation
- Share schema with consumers for early validation
- Avoid code-first APIs that drift from documentation

### 2. Clear and Consistent Naming

- Use resource-based, RESTful naming (e.g., `/fx-orders`, `/positions/{id}`)
- Use kebab-case for paths and camelCase for JSON fields
- Prefix internal APIs with `/internal/` and experimental APIs with `/beta/`

### 3. Versioning and Compatibility

- Always version APIs (e.g., `/v1/`)
- Avoid breaking changes — use additive patterns (new fields, new endpoints)
- Document deprecation schedules and announce at least 2 sprints in advance

### 4. Authentication and Authorization

- All APIs must be protected with JWT or SigV4 tokens
- Use scopes or claims to restrict access to specific resources
- Include `X-User-ID` and `X-Desk-ID` headers where applicable for desk isolation

### 5. Standardized Response Formats

- Responses should follow a common envelope:
  ```json
  {
    "status": "success",
    "data": { ... },
    "traceId": "abc-123",
    "timestamp": "2025-06-07T12:34:56Z"
  }

### 6. Error Handling and Transparency

- Provide machine-readable error codes and human-readable messages
- Use a shared catalog of error codes across services
- Always include `traceId` in error responses

### 7. Observability and Metrics

- Emit structured logs with method, URI, status, latency, and trace ID
- Integrate with CloudWatch dashboards or OpenTelemetry pipelines
- Include correlation IDs across downstream service calls

### 8. Rate Limiting and Fair Usage

- Apply quotas to consumers based on their tier (internal, partner, vendor)
- Return `429 Too Many Requests` with `Retry-After` headers
- Log and alert on abuse or spikes

### 9. Idempotency and Retries

- All `POST` endpoints that mutate state must support idempotency tokens
- Retryable operations should clearly document backoff strategy
- Avoid side effects on GET or HEAD requests

### 10. Documentation and Discoverability

- Every endpoint must be documented in the platform developer portal
- Include example requests/responses and curl examples
- Tag endpoints by business domain, desk, or use case
