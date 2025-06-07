# Data Lake-Specific Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Quarterly or in response to new dataset additions, schema changes, or compliance policies

## Audience

This document is intended for:

- Data Engineers and Platform Architects  
- Analytics and Forecasting Teams  
- ML Engineers and AI Feature Pipeline Owners  
- Governance, Compliance, and Audit Stakeholders

## Governance

- All datasets in the data lake must follow lifecycle policies, schema enforcement, and documented access patterns  
- Data must be classified, discoverable, and registered with metadata and ownership  
- Use cases must be mapped to one of the lake zones (raw, curated, or feature store)

## Automation and Tooling Enablers

- **Data Catalog & Glue Crawlers**: Automate schema discovery and metadata indexing  
- **Access Control via Lake Formation**: Fine-grained permissions by column, table, or tag  
- **Schema Evolution CI Validators**: Block breaking schema changes in pipelines  
- **Storage Class Management**: Tiered lifecycle transitions from S3 Standard to Glacier Deep Archive

---

## Principle Set

### 1. Zone-Based Architecture

- Organize data into zones:  
  - **Raw Zone**: Immutable ingestion with full lineage  
  - **Curated Zone**: Cleaned, deduplicated, analytics-ready datasets  
  - **Feature Zone**: AI-ready, ML-featurized snapshots  
- Data must not move backward between zones (e.g., no curated → raw)

### 2. Schema-On-Read with Validation

- Use schema-on-read engines like Athena or Presto for flexible querying  
- Apply schemas using Glue Catalog or Iceberg table formats for consistency  
- All datasets must pass CI schema validation before ingestion

### 3. Immutable and Auditable Ingestion

- Raw data must be write-once and append-only  
- Use object versioning and hashing to detect tampering  
- Track data source, ingestion time, and batch ID for all records

### 4. Data Lineage and Ownership

- Every dataset must include metadata on owner team, update frequency, and retention policy  
- Enable lineage tracking from source systems to curated/feature outputs  
- Maintain `source_id`, `ingest_batch_id`, and `transform_version` in each record

### 5. PII and Sensitive Data Controls

- Apply Lake Formation tags and column-level access controls for sensitive attributes  
- Mask or tokenize PII fields before exposing to analytics or AI pipelines  
- Enforce encryption-at-rest with CMKs; use VPC endpoints for in-transit security

### 6. Optimized Storage and Partitioning

- Partition datasets by usage-relevant keys (e.g., `desk_id`, `date`, `event_type`)  
- Compact small files regularly using compaction jobs or Iceberg optimization  
- Transition infrequent access data to cheaper storage classes automatically

### 7. Discoverability and Metadata Standards

- All datasets must be registered in the Glue Catalog with searchable metadata  
- Field names, descriptions, and units must follow domain-specific naming conventions  
- Include freshness and update SLA in catalog metadata

### 8. Auditability and Time Travel

- Enable versioning for key curated tables to support rollback and audit analysis  
- Maintain change history and log transform jobs for lineage tracking  
- Archive old versions for a minimum of 12 months

### 9. Compute-Decoupled Querying

- Enable Athena or Trino-based querying for all zones—no raw compute access to buckets  
- All long-running jobs must run with tagged execution context (e.g., team, purpose)

### 10. Governance-by-Design

- Use policy-as-code to enforce ingestion, tagging, and access control requirements  
- Monitor S3 access logs, Glue audit events, and Lake Formation permission changes  
- Include data lake architecture in all system and domain design reviews
