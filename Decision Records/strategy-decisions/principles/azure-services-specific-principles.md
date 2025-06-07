# Azure Services-Specific Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Annually or after Azure architecture shifts, compliance updates, or service onboarding

## Audience

This document is intended for:

- Enterprise Architects and Platform Engineers  
- DevOps and Infrastructure-as-Code Practitioners  
- Application Teams using Azure-based integrations  
- Security, Risk, and Governance Professionals

## Governance

- All Azure services must be aligned with enterprise cloud policies and multi-cloud integration strategy  
- New services must pass platform fit assessment and compliance review before production use  
- Design proposals and architecture decisions using Azure must reference this guideline

## Automation and Tooling Enablers

- **Azure Policy**: Enforce service configuration, tagging, encryption, and region usage  
- **Blueprints & ARM/Bicep Templates**: Standardized deployment scaffolds  
- **Azure Monitor & Log Analytics**: Unified telemetry for Azure-native services  
- **Cost Management Dashboards**: Linked with tags, subscriptions, and resource groups

---

## Principle Set

### 1. Use Platform-Managed Services First

- Prefer services like Azure App Service, Azure Functions, Azure SQL, Cosmos DB, and Azure Data Factory  
- Avoid VM-based custom deployments unless justified for legacy or special needs

### 2. Subscription and Resource Group Structure

- Define clear boundaries for each environment (`dev`, `qa`, `prod`) by subscription or resource group  
- Apply consistent naming and tagging conventions across regions and teams

### 3. Identity and Access Control

- Use Azure Active Directory (AAD) with role-based access control (RBAC)  
- Avoid using shared accounts or unmanaged service principals  
- Assign scoped roles to identities per resource or group

### 4. Encryption and Compliance

- Enable encryption at rest with customer-managed or platform-managed keys (CMK or PMK)  
- Enforce secure transfer (HTTPS, TLS) for data in motion  
- Use Azure Key Vault for secrets, connection strings, and API keys

### 5. Cost and Capacity Optimization

- Use scaling plans, autoscaling rules, and budget alerts for services like Azure Kubernetes Service (AKS), Functions, and SQL  
- Leverage Azure Reservations and Spot VMs where predictable workloads exist  
- Apply cost anomaly detection on daily spend deltas

### 6. Networking and Perimeter Control

- Use Network Security Groups (NSGs), Private Endpoints, and Azure Firewall  
- Avoid public IP exposure unless protected by Azure Front Door or WAF  
- VNet peering and service endpoints must follow least-exposure guidelines

### 7. High Availability and Disaster Recovery

- Deploy across availability zones or paired regions for critical services  
- Enable backups and geo-redundant storage (GRS) for databases and Blob Storage  
- Document RTO and RPO targets per system tier

### 8. Logging, Monitoring, and Tracing

- Enable Diagnostic Logs, Activity Logs, and Azure Monitor for all services  
- Use Application Insights for distributed tracing and telemetry correlation  
- Logs must include `userId`, `operationId`, `env`, and `timestamp`

### 9. Lifecycle Management

- Track service GA/EOL notices and update SDKs, runtimes, and dependencies regularly  
- Use Azure Advisor to detect underutilized resources or misconfigurations  
- Enforce regular reviews of service configurations and policies

### 10. Integration with AWS and Hybrid Workloads

- Use secure identity federation (e.g., AAD ↔ IAM roles), VPN/ExpressRoute, or Azure Arc where applicable  
- Ensure telemetry and policy parity with AWS-native services in multi-cloud flows  
- Document cross-cloud data paths, SLAs, and shared observability expectations
