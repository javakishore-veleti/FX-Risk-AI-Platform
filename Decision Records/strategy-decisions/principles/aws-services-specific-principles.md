# AWS Services-Specific Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Quarterly or following major AWS service updates, billing changes, or architecture reviews

## Audience

This document is intended for:

- Cloud Architects and Platform Engineers  
- DevOps and Infrastructure-as-Code Contributors  
- Application Developers and Team Leads  
- Governance, Security, and FinOps Stakeholders

## Governance

- All AWS service usage must conform to platform-approved patterns and baseline guardrails  
- Deviations or use of experimental services require approval and compensating controls  
- This document is a required input to cloud architecture reviews and cost reviews

## Automation and Tooling Enablers

- **Service Catalog**: Defines allowed, recommended, or restricted AWS services by environment  
- **CI/CD Rulesets**: Prevent provisioning of disallowed configurations or insecure defaults  
- **Guardrails and SCPs**: Prevent use of unsupported regions, services, or access patterns  
- **Monitoring Dashboards**: Aggregate usage, quota breaches, and security findings by service

---

## Principle Set

### 1. Use Managed Services by Default

- Prefer managed options (e.g., RDS, Lambda, ECS Fargate, SQS, S3) over self-managed EC2 setups  
- Justify any infrastructure running on EC2 or custom Kubernetes clusters

### 2. Region and Availability-Aware Design

- Deploy into platform-approved AWS regions only  
- Services with high availability requirements must span multiple AZs  
- Services requiring global data access (e.g., S3, CloudFront) must follow data residency policy

### 3. Identity and Access Management

- All AWS service interactions must use scoped IAM roles and policies  
- No use of long-lived credentials or hardcoded access keys  
- Services must assume roles with limited, context-aware permissions

### 4. Encryption and Secret Handling

- All supported AWS services must use AWS KMS for encryption at rest  
- In-transit encryption must be enabled via HTTPS, TLS 1.2+  
- Secrets must be stored in AWS Secrets Manager or SSM Parameter Store

### 5. Cost-Conscious Configuration

- Use autoscaling, spot instances, or serverless where feasible  
- Apply lifecycle policies for S3, Redshift, OpenSearch, and CloudWatch logs  
- Avoid overprovisioning by selecting the right service tier (e.g., RDS Aurora Serverless v2 over provisioned)

### 6. Observability and Logging Integration

- Every AWS service used must emit logs to CloudWatch and metrics to dashboards  
- Use structured logging and correlate with `traceId`, `env`, and `service` tags  
- Enable detailed monitoring where required for production

### 7. Quota and Fault Monitoring

- Services must monitor usage against AWS service quotas (e.g., Lambda concurrency, S3 object limits)  
- Set alarms for thresholds nearing quota or performance degradation  
- Include fallback logic or queuing when rate limits are breached

### 8. Network Architecture Alignment

- All services must deploy into approved VPCs and subnets  
- Public endpoints must be approved and protected by WAF, API Gateway, or CloudFront  
- Use VPC endpoints or PrivateLink where feasible for secure internal traffic

### 9. Service-Level DR and BCP

- Critical services (RDS, S3, Lambda) must be configured for multi-AZ or cross-region backups  
- Snapshots, lifecycle policies, and restore plans must be documented  
- S3 buckets should enable versioning and MFA delete for sensitive data

### 10. Upgrade and Lifecycle Awareness

- Monitor AWS service deprecation notices and versioning timelines  
- Schedule regular reviews of service configurations (e.g., Lambda runtimes, RDS engine versions)  
- Leverage AWS Trusted Advisor and Security Hub findings proactively
