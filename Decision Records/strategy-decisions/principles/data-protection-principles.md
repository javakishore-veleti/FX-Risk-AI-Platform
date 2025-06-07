# Data Protection Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform
- **Review Cadence**: Semi-annually or in alignment with regulatory, security, or audit reviews

## Audience

This document is intended for:

- Data Engineers and Architects  
- Security and Compliance Officers  
- Platform and Application Engineers  
- DevOps and SRE Teams

## Governance

- All services and data flows must conform to these principles before being promoted to production  
- Violations require written risk acceptance and audit tracking  
- Data classification, protection, and access must be part of every architecture review

## Automation and Tooling Enablers

- **Automated Classification**: Tag sensitive fields using tools like Macie or manual tagging pipelines  
- **Access Policy Audits**: Periodic IAM and Lake Formation policy scans for over-permissioned access  
- **Redaction & Masking Services**: Shared utility for on-the-fly PII/PCI redaction and tokenization  
- **Logging & Traceability**: All data accesses logged with user, action, timestamp, and purpose

---

## Principle Set

### 1. Classify Data Early

- Tag data assets as PII, PCI, confidential, or public during ingestion and cataloging  
- Apply downstream protections based on classification level

### 2. Encrypt by Default

- Encrypt all data at rest using AWS KMS (S3, RDS, Redshift, Glue)  
- Encrypt data in transit with TLS 1.2 or higher  
- Audit key usage, rotation, and access scope

### 3. Principle of Least Access

- Grant only the minimum permissions needed (IAM roles, Lake Formation, resource policies)  
- Prohibit blanket access to entire buckets, tables, or datasets

### 4. Fine-Grained Access Control

- Use tag-based or attribute-based access control for S3, Athena, and Glue  
- Apply desk, role, and region scoping to data access wherever feasible

### 5. Audit Every Access

- Every read/write must be logged with `who`, `what`, `when`, and `why`  
- Use CloudTrail, Lake Formation logs, and Redshift audit logs for traceability

### 6. Mask or Tokenize Sensitive Fields

- Mask PII and secrets in logs, APIs, UI, and downstream outputs  
- Use shared utilities or AWS Macie-compatible rules for identifying sensitive fields

### 7. Retention and Deletion Policies

- Data must be retained and purged per business and regulatory timelines  
- Lifecycle policies must be attached to S3, Redshift, OpenSearch, and backups

### 8. Data Localization and Residency

- Ensure data is stored and processed in allowed AWS regions  
- Use region-scoped resources and policies for compliance-sensitive workloads

### 9. Secure Sharing and Export

- Prohibit ad hoc exports of sensitive datasets without justification and audit  
- Apply tagging, watermarking, or row-level access controls before data sharing

### 10. Privacy-By-Design

- Design systems to avoid storing or exposing unnecessary personal data  
- Limit collection to what’s necessary for the workflow; anonymize when possible
