# Cloud Governance Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Annually or upon significant changes in AWS policies, audit findings, or cost trends

## Audience

This document is intended for:

- Cloud Infrastructure and DevOps Engineers  
- Security and Compliance Officers  
- Engineering Managers and Application Teams  
- Cloud Account Owners and FinOps Leads

## Governance

- All cloud accounts, resources, and services must comply with the principles outlined here  
- Deviations require risk documentation and quarterly exception reviews  
- Infrastructure reviews and audits will assess alignment against governance domains

## Automation and Tooling Enablers

- **Tag Policies**: Enforce mandatory metadata (e.g., `owner`, `costCenter`, `env`, `region`)  
- **AWS Config Rules**: Detect violations (e.g., open ports, unencrypted volumes)  
- **Guardrails via SCPs**: Prevent disallowed services or regions  
- **FinOps Dashboards**: Cost explorer reports, usage trends, and budget alerts  
- **IAM Access Analyzer**: Identify overly permissive or unused permissions

---

## Principle Set

### 1. Account and Environment Isolation

- Use separate AWS accounts for prod, staging, and dev environments  
- Each business domain or capability should map to a clearly owned account or VPC  
- Sandbox accounts must have spending and access limits

### 2. Standardized Tagging

- All cloud resources must be tagged with owner, purpose, environment, cost center, and lifecycle stage  
- Untagged resources are automatically flagged for remediation or termination

### 3. Least Privilege and Access Boundaries

- IAM roles must be scoped to the minimum required permissions  
- Use SCPs and permission boundaries to enforce limits at the org/account level  
- Access to production must require approval and logging

### 4. Secure-by-Default Configuration

- Deny public access to S3, RDS, Redshift unless explicitly reviewed  
- Enforce encryption at rest with KMS and TLS in transit  
- Use private subnets for non-internet-facing workloads

### 5. Resource Lifecycle and Hygiene

- Apply lifecycle policies for unused volumes, logs, and snapshots  
- Auto-stop or destroy idle dev environments outside working hours  
- Archive old backups per data retention policy

### 6. Budgeting and Cost Visibility

- Set monthly budgets for each account or team with alert thresholds  
- Use cost allocation tags for dashboards and anomaly detection  
- Reserved instance and savings plan coverage must be reviewed quarterly

### 7. Policy-as-Code

- Cloud governance controls must be implemented as code (e.g., Terraform, CDK, OPA)  
- Git-based workflows and PR reviews enforce changes to infrastructure policies  
- Prevent drift using AWS Config and regular audits

### 8. Auditability and Traceability

- CloudTrail and Config must be enabled in all accounts  
- All changes must be traceable to a user, service role, or automation pipeline  
- Dashboards must show non-compliant resources and their owners

### 9. Service Approval Process

- New AWS services must go through a vetting process before use in production  
- Maintain an allowlist/blocklist by account, region, or use case  
- Monitor shadow IT and rogue resource creation

### 10. Shared Responsibility Awareness

- Teams must understand AWS’s shared responsibility model  
- Application owners are accountable for their data, network config, and IAM logic  
- Platform team provides hardened scaffolds, policy enforcement, and visibility tooling
