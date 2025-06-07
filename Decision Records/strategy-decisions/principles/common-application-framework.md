# Common Application Framework (CAF) — Principles and Practices

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform
- **Review Cadence**: Quarterly or aligned with platform refactors or shared library upgrades

## Audience

This document is intended for:

- Application Developers
- Tech Leads and Engineering Managers
- Platform and Framework Maintainers
- SREs and DevOps Teams

## Governance

- All new services or modules must either use or justify deviation from the CAF libraries and scaffolds
- Architecture reviews evaluate reuse of shared packages, orchestration patterns, and observability tooling
- Customizations must not compromise cross-team interoperability

## Automation and Tooling Enablers

- **Scaffold CLI**: Bootstrap new services with language-specific templates (e.g., FastAPI, Angular)
- **Shared Libs**: Versioned libraries in `fx-risk-ai-shared-libs` for auth, logging, error handling
- **Code Quality Gates**: Enforce dependency hygiene and framework usage via CI checks
- **CAF Registry**: Documented modules, owners, release history, and usage examples

---

## Framework Principles

### 1. Convention Over Configuration

- All services share standard folder structure, naming conventions, and logging formats
- Scaffold CLI sets up repo and service skeletons with opinionated defaults

### 2. Opinionated Stack, Pluggable Layers

- Default to FastAPI (Python), Angular (UI), and AWS Lambda (or Fargate) for service layers
- Allow plug-in mechanisms for custom routing, async jobs, and third-party APIs

### 3. Consistent Auth and Identity Propagation

- Use centralized JWT/IAM token verification module
- All services extract and log userId, deskId, and role from token headers

### 4. Unified Error Handling

- Common exception classes, response formats, and trace propagation
- No raw 500s — errors must include code, message, correlationId, and hints

### 5. Built-In Observability

- Standardized logger and metric decorators in shared library
- Automatic injection of `traceId`, request path, latency, and outcome

### 6. Domain-Driven Modularity

- Each CAF service should map to a business capability (e.g., Forecasting, Audit, Exception Triage)
- Modules must support independent deployment and versioning

### 7. Testability by Design

- Include auto-generated test stubs with scaffolds
- All services support local mocking, fixture loading, and contract testing

### 8. Scaffold First, Then Customize

- Never build from scratch — use CLI tools or copy existing CAF service
- Customizations must document reasons and not compromise maintainability

### 9. Shared Data Models and DTOs

- Use centralized `fx-risk-ai-shared-libs` for commonly used schemas (e.g., exceptions, audit log events)
- Avoid duplicating business object definitions across services

### 10. API and UI Coherence

- All APIs follow platform API guidelines and error spec
- Shared Angular components for tables, modals, routing, and API integration

---

## Summary

The Common Application Framework enables:

- Faster onboarding of new teams and services  
- Consistent developer experience and runtime behavior  
- Reduced maintenance through shared abstractions and tooling  

It is a **strategic asset** for scaling productivity, quality, and architecture consistency across the platform.
