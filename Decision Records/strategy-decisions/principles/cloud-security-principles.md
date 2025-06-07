# Cloud Security Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform
- **Review Cadence**: Semi-annually or upon any significant change in cloud posture, audit, or compliance requirements

## Audience

This document is intended for:

- Security Engineers and IAM Owners
- Cloud Infrastructure and DevOps Teams
- Application Developers and Architects
- Legal, Compliance, and Risk Auditors

## Governance

- All services must comply with these principles before production release
- Reviews are conducted during architecture reviews, security assessments, and internal audits
- Violations require written risk exceptions, reviewed quarterly

## Automation and Tooling Enablers

- **AWS Config Rules**: Detect misconfigured resources (e.g., open S3 buckets, unencrypted EBS)
- **IAM Policy Scanners**: Check for privilege escalation risks and over-permissive roles
- **CI/CD Hooks**: Fail builds for missing encryption, open ports, or shared credentials
- **Secrets Management**: Mandatory use of AWS Secrets Manager or Parameter Store
- **Centralized Logging**: Enforced through CloudTrail, GuardDuty, and AWS Organizations

---

## Principle Set

### 1. Least Privilege Access

- Grant minimum necessary permissions for all users, services, and roles
- Avoid wildcards (`*`) in IAM policies unless scoped by condition keys

### 2. Encrypt Everywhere

- All data at rest must use AWS-managed or customer-managed KMS keys
- All data in transit must use TLS 1.2 or higher
- Logs, backups, and cache layers must also be encrypted

### 3. Isolate Environments and Tenants

- Use separate AWS accounts or VPCs for dev/staging/prod
- Tenant data must be logically or physically isolated, with enforced IAM boundaries

### 4. Secure Secrets Handling

- Never store credentials or tokens in source code or environment files
- Use dynamic secret retrieval from AWS Secrets Manager with audit logging

### 5. Public Access Denied by Default

- All internet-facing endpoints must be explicitly reviewed and approved
- S3 buckets, RDS instances, and OpenSearch must deny public access unless required

### 6. Auditable Everything

- All actions must be logged via CloudTrail, including admin actions, API calls, and console logins
- Access logs must include identity, time, region, and action

### 7. Immutable Infrastructure

- Deploy from secure, versioned templates; never patch or edit resources manually
- Logs and metrics must be retained according to audit timelines

### 8. Zero Trust Networking

- Apply deny-by-default VPC and security group policies
- Use VPC endpoints, NACLs, and private link patterns for inter-service communication

### 9. Identity-Aware Controls

- Enforce MFA for human access to sensitive environments
- Apply IAM conditions to restrict actions by tag, IP, or resource

### 10. Continuous Monitoring and Remediation

- Enable GuardDuty, Security Hub, and Config Rules organization-wide
- Triage and respond to findings weekly; critical alerts must page the on-call team
