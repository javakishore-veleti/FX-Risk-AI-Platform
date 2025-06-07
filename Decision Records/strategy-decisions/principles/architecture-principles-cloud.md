# Cloud Architecture Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform
- **Review Cadence**: Annual, or aligned with major AWS architectural changes or cost reviews

## Audience

This document is intended for:

- Cloud Architects
- DevOps and SRE Teams
- Security Engineers
- Infrastructure Leads
- Domain and Platform Engineers

## Governance

- All infrastructure and service deployments must comply with these principles.
- Cloud cost, resilience, and security must be evaluated in architecture reviews.
- Violations must be captured in ADRs and linked to mitigation timelines.

## Automation and Tooling Enablers

- **Infrastructure-as-Code**: All infra must be provisioned via CDK/Terraform with policy checks
- **Cost Allocation Tags**: Required for every deployed AWS resource
- **Security Scanners**: Enforce IAM least privilege, encryption, and network isolation
- **Well-Architected Review (WAR)**: Mandatory for high-risk workloads or after major infra changes

---

## Principle Set

### 1. Cloud-Native First

- Prioritize managed AWS services over self-managed infrastructure.
- Reduce operational overhead by using native integrations (e.g., EventBridge, Step Functions).

### 2. Resilience Through Redundancy

- All services must be Multi-AZ by default.
- Critical stateful workloads (e.g., RDS, OpenSearch) must have failover configurations.

### 3. Infrastructure as Code (IaC)

- All resources must be deployed and version-controlled via IaC.
- Manual console deployments are prohibited in production environments.

### 4. Secure by Design

- Apply IAM roles with least privilege access.
- Block public access to S3, RDS, and OpenSearch unless explicitly reviewed.
- Rotate secrets using AWS Secrets Manager or Parameter Store.

### 5. Observability and Auditability

- Enable CloudTrail, GuardDuty, and Config Rules for all accounts.
- Ensure structured logs, metrics, and traces are captured centrally.
- Tag all resources with `environment`, `owner`, and `service` metadata.

### 6. Cost Transparency

- All services must emit cost-attribution tags.
- Cost Explorer reports must be reviewed monthly by tech leads.
- Budgets and alarms must be set for each environment.

### 7. Auto Scaling and Elasticity

- All compute services must scale with load — use Lambda, Fargate, or EC2 Auto Scaling.
- Define upper/lower bounds for scale events in prod and non-prod environments.

### 8. Immutable Infrastructure

- New deployments should not modify existing instances — redeploy from templates.
- Use blue/green or canary deployment strategies for critical services.

### 9. Environment Separation

- Separate accounts or VPCs for dev, staging, and production.
- No shared state or resources across environments except via secure APIs.

### 10. Zero Trust Networking

- Default deny at network and IAM level — allow only explicit ports/services.
- Use VPC endpoints, PrivateLink, and NACLs to restrict access scope.
