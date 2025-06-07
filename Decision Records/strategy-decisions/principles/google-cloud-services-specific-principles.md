# Google Cloud Services-Specific Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Annually or following major GCP service adoption, architecture changes, or compliance policy updates

## Audience

This document is intended for:

- Cloud Architects and GCP Engineers  
- DevOps and Site Reliability Teams  
- Application Developers integrating GCP services  
- Security, Audit, and Compliance Leads

## Governance

- All GCP usage must align with enterprise cloud strategy, security policy, and financial guardrails  
- New GCP service usage must undergo architecture review and cost/governance impact assessment  
- Interoperability with AWS or Azure services must be documented and approved

## Automation and Tooling Enablers

- **Organization Policies**: Control regions, encryption, allowed services, and access models  
- **Cloud Logging & Monitoring**: Stackdriver for logs, metrics, alerts, and dashboards  
- **Deployment Manager or Terraform**: IaC templates to enforce consistent GCP provisioning  
- **Cloud Asset Inventory & Security Command Center**: Audit resource states and misconfigurations

---

## Principle Set

### 1. Use Fully Managed GCP Services by Default

- Prioritize Cloud Run, Cloud Functions, App Engine, BigQuery, Pub/Sub, and Firestore over custom compute  
- Minimize use of unmanaged VMs and avoid persistent shell-based workflows

### 2. Project-Level Isolation and Tagging

- Each environment (e.g., dev, test, prod) must reside in isolated GCP projects  
- Use labels for `owner`, `team`, `environment`, `application`, and `cost-center`  
- Apply IAM at project and resource level using the principle of least privilege

### 3. Identity and Access Management

- Use Google Groups and IAM roles tied to managed identities (no personal Gmail or wide-scoped access)  
- Rotate service account keys automatically and restrict impersonation rights  
- Enable IAM audit logging on all projects and sensitive resources

### 4. Encryption and Key Management

- Enforce encryption at rest using CMEK or Google-managed keys where appropriate  
- Use TLS 1.2+ for data in transit; do not allow plaintext service-to-service communication  
- Store and manage secrets with Secret Manager, not inline variables or config files

### 5. Network Design and Zero Trust Principles

- Use VPC Service Controls for data boundary enforcement  
- Minimize public IP exposure — prefer private service connect and internal load balancers  
- Use Identity-Aware Proxy (IAP) to secure internal web services and endpoints

### 6. Logging, Monitoring, and Observability

- Enable Cloud Monitoring and Logging for all services (e.g., GKE, BigQuery, Cloud Run)  
- Implement request tracing using Cloud Trace or OpenTelemetry  
- Instrument metrics and logs with labels such as `project_id`, `region`, `trace_id`, and `request_id`

### 7. Billing Control and Quota Awareness

- Set per-project budgets with alerts for 80%, 100%, and 120% thresholds  
- Use committed use discounts (CUDs) and workload rightsizing tools  
- Disable unused APIs and set quotas explicitly to prevent overrun

### 8. Data Services and Pipelines

- Use BigQuery for serverless analytics workloads; avoid misuse of expensive on-demand scans  
- Use Dataflow for ETL/streaming with schema enforcement  
- Datasets must be versioned and documented with access policies

### 9. Security Best Practices

- Enable Security Command Center and set severity-driven response playbooks  
- Audit access to GCS, BigQuery, and Pub/Sub via centralized dashboards  
- Set organization-level constraints on sensitive APIs and storage classes

### 10. Multi-Cloud and Interoperability

- Use Pub/Sub, BigQuery Federated Queries, and GKE Autopilot where cross-cloud integration is needed  
- Document data flows between GCP and AWS or Azure, with SLA, latency, and cost profiles  
- Unify monitoring via exporters or third-party observability platforms
