# DevOps Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Quarterly or in conjunction with major CI/CD, infrastructure, or security changes

## Audience

This document is intended for:

- DevOps Engineers and SREs  
- Platform Engineers and Infrastructure Leads  
- Tech Leads and CI/CD Pipeline Maintainers  
- Application Developers integrating services with the platform

## Governance

- All services must adhere to these principles before production deployment  
- Pipelines must validate service health, security posture, test coverage, and deployment policy  
- Violations must be documented and mitigated with compensating controls or exception handling

## Automation and Tooling Enablers

- **CI/CD Pipelines**: Standardized GitHub Actions and AWS CodePipeline templates  
- **Security Gates**: Tools like Bandit, Trivy, Checkov, and OPA run pre-merge  
- **Deployment Strategies**: Canary, blue-green, and feature-flag-based rollouts  
- **Observability Hooks**: Auto-publish metrics, traces, and deployment events to dashboards

---

## Principle Set

### 1. Everything as Code

- Infrastructure (CDK/Terraform), policies, dashboards, and pipeline logic must be versioned  
- Avoid manual deployments, inline scripts, or undocumented procedures

### 2. Shift Left on Security

- Embed security scanning, policy checks, and permission analysis in CI  
- Enforce fail-fast rules for public access, plaintext secrets, and unscanned containers

### 3. Continuous Integration and Delivery

- All pull requests must trigger builds, tests, linting, and vulnerability scanning  
- Merge only on successful CI validation; use trunk-based development or GitFlow where applicable

### 4. Immutable Infrastructure and Deployments

- All services must be deployed as versioned artifacts (containers, Lambdas)  
- Never patch infrastructure in-place — replace with new versions

### 5. Progressive Exposure

- Use feature flags, A/B rollouts, and phased releases for risky changes  
- Validate success criteria (error rate, latency, feedback) before scaling to all users

### 6. Observability-Driven Deployment

- All services must publish deploy events, traces, and alerts to shared observability stack  
- Deployment failures should auto-trigger rollbacks and notify responsible teams

### 7. Secrets and Config Separation

- Secrets must be stored in AWS Secrets Manager or SSM Parameter Store  
- Config must be externalized and environment-specific (not hardcoded)

### 8. Standardized CI/CD Templates

- Use shared templates and build workflows across teams  
- Promote reuse of testing steps, quality gates, and deployment workflows

### 9. Resilient by Design

- All deployments must include retry logic, timeouts, and fallback routing  
- Rollback plans must be defined and testable for every environment

### 10. Developer Experience Matters

- Fast pipelines (<10 min CI target), clear logs, and actionable errors  
- Empower teams to self-serve scaffolds, environment bootstraps, and deployment previews
