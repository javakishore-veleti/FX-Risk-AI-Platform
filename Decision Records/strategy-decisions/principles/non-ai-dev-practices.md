# Non-AI Development Practices

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Semi-annually or following platform-level refactors or language/stack upgrades

## Audience

This document is intended for:

- Application Developers and Engineering Leads  
- DevOps Engineers and QA Teams  
- Architects and Platform Engineers  
- Backend, Frontend, and Integration Developers

## Governance

- All code submitted to Fx-Risk-AI platform services must adhere to these practices  
- Engineering reviews and CI pipelines enforce structure, coverage, linting, and security gates  
- PR templates and scaffolds will be updated to reflect these standards

## Automation and Tooling Enablers

- **CI Linting**: Black, Flake8 (Python); ESLint/Prettier (Angular)  
- **Type Checking**: MyPy (Python); TypeScript enforcement (UI)  
- **Test Coverage Enforcement**: pytest-cov, Jest, SonarCloud thresholds  
- **Static Analysis**: Bandit (Python), Trivy (container), CodeQL (GitHub)  
- **PR Templates**: Checklist-driven development with reviewer accountability

---

## Development Principles

### 1. Clean, Modular Codebase

- Each module must serve a single purpose and follow separation of concerns  
- Folder structures must reflect logical layers (e.g., `api`, `services`, `schemas`, `utils`)

### 2. Type-Safe by Default

- All functions must include type hints (Python) or use TypeScript (JS/TS)  
- CI will block submissions with type inference failures or skipped coverage

### 3. Test-First or Test-Parity

- Unit tests must accompany all non-trivial modules and utility functions  
- Test coverage targets: 80% for core modules; 100% for shared libs and utilities

### 4. Explicit Error Handling

- Avoid silent exceptions or generic `except:` blocks  
- Use shared error response formats (`code`, `message`, `traceId`) in APIs

### 5. DRY, But Documented

- Don’t Repeat Yourself (DRY) applies to logic, not to clarity  
- Favor readable, maintainable code over overly abstract helpers

### 6. Minimal External Dependencies

- Use external libraries only when they offer tested, stable, and necessary functionality  
- Prefer platform-vetted libraries documented in the shared developer guide

### 7. Observability First

- Log every service entry point and exit with `traceId`, status, and latency  
- Emit metrics for core workflows using decorators or standard patterns

### 8. Safe Defaults and Idempotency

- APIs and jobs must be safe to retry; support `Idempotency-Key` where state changes occur  
- All API routes must return consistent JSON schemas (even on errors)

### 9. Secure Code Practices

- Validate input at boundaries — no implicit trust of user input  
- Sanitize logs to prevent injection and redact PII  
- Follow shared guidance on encryption, secrets, and access control

### 10. Developer Productivity

- Favor opinionated scaffolds for speed and consistency  
- Run `pre-commit` hooks locally; CI enforces identical checks  
- Provide `Makefile` or task runner support for common workflows (`make test`, `make lint`, etc.)
