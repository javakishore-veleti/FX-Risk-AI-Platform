# DevSecOps Policy Alignment

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Semi-annually or after major changes in regulatory posture, CI/CD tooling, or AWS security services

## Audience

This document is intended for:

- Security Engineers and Governance Leads  
- DevOps and SRE Teams  
- Application Developers and Tech Leads  
- Platform and Cloud Architects

## Governance

- All services must comply with DevSecOps controls prior to production deployment  
- Pipelines enforce security gates and produce evidence of compliance  
- Exceptions must be documented and reviewed quarterly by a governance committee

## Automation and Tooling Enablers

- **CI Security Scanners**: Bandit, Trivy, Semgrep for code and container scanning  
- **Infrastructure Policy-as-Code**: Checkov, tfsec, and OPA applied at pre-deployment  
- **Secrets Enforcement**: Git hooks + CI plugins to prevent plaintext credential commits  
- **Runtime Observability**: AWS GuardDuty, Config, and Security Hub aggregation  
- **Audit Trail Aggregation**: Centralized CloudTrail logs for all infrastructure and application activity

---

## Policy Alignment Areas

### 1. Code Security

- All source code must pass static code analysis before merge  
- Vulnerabilities above severity threshold (e.g., critical/high) block pipeline execution  
- Approved linters and security rulesets are standardized across language stacks

### 2. Secrets Management

- Secrets may never be stored in code, environment files, or Git history  
- Use AWS Secrets Manager or SSM Parameter Store for encrypted storage and versioning  
- Rotations must be automated where supported by the target system

### 3. Container and Dependency Hygiene

- All containers must be built from hardened base images  
- Vulnerability scans must occur on every build and pull request  
- Use artifact signing and digest locks to prevent image spoofing

### 4. IAM and Role Policies

- All roles must follow least privilege and be scoped by service, desk, or function  
- Use role assumption instead of access keys; enforce MFA for console use  
- IAM diffs must be reviewed during pull request or infra plan stage

### 5. Network Security

- All services must restrict ingress/egress via security groups and NACLs  
- Default-deny posture must be enforced at VPC and service layer  
- Use VPC endpoints and PrivateLink for internal communications

### 6. Deployment Safety

- CI/CD pipelines must include dry runs, validation, and rollback strategy  
- Approvals are required for production rollouts of sensitive services  
- All deployments are logged and associated with traceable build artifacts

### 7. Runtime Threat Detection

- AWS GuardDuty, Shield, and Inspector must be enabled in production accounts  
- Alerts must route to security and SRE teams with severity tagging  
- Weekly audits must verify no disabled controls or skipped alerts

### 8. Data Security

- All data at rest and in transit must be encrypted (TLS 1.2+, KMS-managed keys)  
- PII and sensitive fields must be masked in logs and dashboards  
- S3, Redshift, and RDS public access must be blocked by default

### 9. Logging and Auditability

- Every service action, deployment, and access must be traceable to a user or role  
- CloudTrail, ALB, and Lambda logs must be centralized with retention configured  
- Dashboards and alerts must flag suspicious or anomalous access

### 10. Policy-as-Code and Drift Detection

- Use policy scanners to fail builds with misconfigurations or escalations  
- Infra drift must be detected using tools like AWS Config and Terraform state checks  
- Report drift metrics weekly and tag any unapproved changes
