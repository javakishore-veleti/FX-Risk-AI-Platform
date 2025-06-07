# Feature Store Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Semi-annually or with each major ML pipeline revision

## Audience

- ML Engineers and Feature Engineers  
- Platform Engineers and Data Engineers  
- QA and MLOps Teams

## Purpose

Guide consistent feature engineering, reuse, storage, and governance practices across AI/ML models in the platform using a centralized feature store.

---

## Principle Set

### 1. Centralized Feature Registry

- All features must be registered with metadata:
  - `name`, `description`, `team`, `owner`  
  - `entity_id`, `data_type`, `default`, `last_updated`

### 2. Online and Offline Parity

- Ensure that real-time and batch values for the same feature match  
- Validate timestamp alignment and transformation consistency

### 3. Entity-Centric Design

- Features must reference unique entities (`ticket_id`, `user_id`, `case_id`)  
- Avoid joining across unrelated entities in real time

### 4. Feature Lineage and Provenance

- Store source table, transformation logic, and refresh schedule  
- Use Git for transformation versioning (e.g., dbt, PySpark jobs)

### 5. Feature Reusability and Namespacing

- Prefix features with domain (e.g., `audit_avg_latency`, `forecast_volatility_score`)  
- Reuse canonical definitions for shared metrics

### 6. Access Controls

- Use role-based permissions for feature groups  
- Prevent editing of production features without peer review

### 7. Drift Monitoring and Validation

- Auto-monitor value distribution, nulls, and outliers  
- Alert when feature drifts from training distribution

### 8. Data Freshness SLAs

- Label features as `real-time`, `hourly`, `daily`, or `adhoc`  
- Refresh cadence must be visible and enforceable

### 9. Feature Set Snapshots for Training

- Snapshot features at training time with point-in-time correctness  
- Avoid target leakage using temporal joins or time-travel

### 10. Explainability Integration

- Each feature must support business explanation  
- Link feature definitions to dashboards or notebooks for traceability
