# ODS (Operational Data Store) Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Quarterly or when new operational source systems or near-real-time APIs are introduced

## Audience

This document is intended for:

- Backend Engineers and System Integrators  
- Platform and Data Architects  
- Workflow Designers and Product Owners  
- QA, SRE, and Support Teams who rely on timely operational data

## Governance

- The ODS serves as the system-of-record proxy for operational decisioning and workflow resolution  
- It must reflect source-of-truth systems in near real-time, without duplication or redefinition of business logic  
- ODS schemas, sync mechanisms, and access patterns must be versioned and auditable

## Automation and Tooling Enablers

- **Change Data Capture (CDC)** pipelines (e.g., Debezium, DMS) for low-latency sync  
- **Schema Registry** for ODS table contracts  
- **Read Replicas and Caching** for low-latency API integration  
- **CI Guards** for schema changes, constraint validations, and referential integrity

---

## Principle Set

### 1. Reflect Operational Reality, Not History

- ODS tables must represent current operational states (e.g., open tickets, active disputes, pending approvals)  
- Do not store historical snapshots—use the warehouse or lake for that purpose  
- Use soft deletes or status fields to reflect lifecycle transitions

### 2. Source-of-Truth Aligned

- Every ODS entity must be sourced from a defined authoritative system (e.g., claims processor, treasury queue)  
- The ODS must not derive new business logic or perform aggregations beyond basic joins/denormalization

### 3. Low-Latency and Near Real-Time

- Synchronization latency should be within 1–5 minutes for most domains  
- Use CDC or streaming where push-based integration is available  
- Monitor freshness with automated alerts and dashboards

### 4. Idempotent and Reversible Loads

- All data syncs must be idempotent and replay-safe  
- Each record must include metadata: `source_id`, `last_updated_at`, `sync_batch_id`  
- Support rollback or patching of out-of-sync records through validation jobs

### 5. Canonical Schemas and Naming

- All entities must follow naming conventions shared across domains (e.g., `ticket`, `exception_case`, `audit_event`)  
- Field names should match source systems or use approved mapping in a shared dictionary  
- Track schema evolution using migration scripts and changelogs

### 6. API-First Data Access

- ODS should power RESTful or GraphQL APIs with field-level control and pagination  
- All exposed APIs must enforce RBAC and rate limiting  
- Avoid direct table access from external systems unless explicitly documented

### 7. Referential Integrity and Constraints

- Enforce foreign keys, uniqueness, and required fields within the ODS schema  
- Validate cardinality and lookup references against master data tables  
- Prefer denormalized models only when justified for latency

### 8. SLA and Reconciliation Monitoring

- Set SLAs for ODS record freshness, sync duration, and availability  
- Run regular reconciliation reports against source-of-truth systems  
- Track drift or sync errors by domain and notify owning teams

### 9. Multi-Tenant and Domain-Aligned

- Partition or filter data by desk/team where applicable (e.g., `desk_id`, `region`)  
- Ensure isolation of domain ownership and permissioned access for shared tables

### 10. Auditability and Change Tracking

- Track all mutations via CDC logs, metadata columns, or journaling tables  
- Include `created_at`, `updated_at`, `source_event_id`, and `change_type` in each record  
- Make data traceable back to original system events and ingest flows
