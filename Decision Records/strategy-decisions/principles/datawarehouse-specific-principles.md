# Data Warehouse-Specific Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Biannually or when major schema, volume, or analytics workload changes are introduced

## Audience

This document is intended for:

- Data Engineers and Analytics Architects  
- BI Analysts and Reporting Developers  
- Forecasting and Treasury Data Consumers  
- Governance, Risk, and Audit Teams

## Governance

- All datasets in the warehouse must follow schema design standards, access controls, and performance SLAs  
- Dashboards, queries, and reports must be built on curated, approved views or materialized tables  
- Warehousing systems must align with data retention and compliance mandates

## Automation and Tooling Enablers

- **ETL Pipelines (e.g., AWS Glue, dbt)**: Enforce naming, transformations, lineage  
- **Data Quality Monitors**: Test row counts, nulls, constraints, and distribution drifts  
- **BI Metadata Sync**: Integrate field-level lineage into QuickSight or Tableau  
- **Role-Based Access Control (RBAC)**: Column- or row-level permissions in Redshift or Snowflake

---

## Principle Set

### 1. Star or Snowflake Schemas for Analytics

- Design fact and dimension tables with clear grain and consistent surrogate keys  
- Use slowly changing dimensions (SCD Type 1 or 2) where needed  
- All joins should be documented and reflected in data dictionary

### 2. Curated Views, Not Ad Hoc Tables

- All BI or operational queries must be built on version-controlled views or marts  
- Ad hoc data should be materialized and validated before exposure to dashboards

### 3. Centralized Metric Logic

- Key metrics (e.g., forecast accuracy, audit turnaround time) must be defined centrally  
- Avoid duplication of business logic across tools or queries  
- Metrics must include definition, granularity, and responsible team

### 4. Performance-Aware Design

- Partition fact tables by time or business unit for pruning  
- Apply sort keys, materialized views, and compression settings based on query patterns  
- Use query profiling tools to monitor cost and optimize access

### 5. Near-Real-Time ETL Where Required

- SLA-sensitive datasets must use incremental loads or micro-batching (e.g., every 5–10 minutes)  
- Late-arriving data must be flagged and corrected without backfilling entire tables

### 6. Access Control and Compliance

- Sensitive attributes must be masked, redacted, or permissioned by role  
- Row-level security must enforce desk, user, or geography isolation  
- Use column-level RBAC and audit logs for every query run on regulated datasets

### 7. Semantic Layer Governance

- Maintain a curated semantic model or data mart layer per domain (e.g., Forecasting, Exceptions, Audit)  
- Avoid “wild west” dashboards with direct table access  
- Enforce metric naming, folder hierarchy, and field descriptions in BI tools

### 8. Source of Truth Alignment

- All warehouse entities must be traceable to raw and curated lake sources  
- Warehouse-derived KPIs must align with domain owners and match operational definitions  
- Data lineage tools must map end-to-end flow for auditing

### 9. Retention and Storage Efficiency

- Archive old partitions or cold marts based on business needs  
- Use tiered storage and auto-vacuum settings (e.g., in Redshift)  
- Clean up orphaned tables, snapshots, or outdated temp marts quarterly

### 10. Change Management and Versioning

- Schema changes must go through formal review and be documented  
- Track SQL or model version in dashboards and notebooks  
- Include lineage metadata (e.g., source table, transform job, last refresh time) in all exposed views
