# Apache Spark, Flink, and EMR Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Biannually or after major changes in stream/batch architecture, cost optimization mandates, or AWS ecosystem updates

## Audience

This document is intended for:

- Data Engineers, ML Engineers, and ETL Developers  
- Platform and Analytics Architects  
- DevOps, Infrastructure, and FinOps Teams  
- AI Workflow and Feature Engineering Specialists

## Governance

- All Spark/Flink/EMR workloads must be tagged, version-controlled, resource-managed, and observable  
- Jobs must adhere to security, cost, and retry guidelines defined by platform engineering  
- Architecture decisions involving these engines must be captured in ADRs and aligned with platform data flow standards

---

## Section 1: Apache Spark Principles

### 1. Use Cases for Spark

- Use Spark for:
  - High-volume batch ETL workloads (e.g., forecasting snapshots, audit trails)
  - Feature generation from data lake zones  
  - Large-scale joins and windowed aggregations  
  - ML model training using distributed DataFrames

### 2. Job Design & Scalability

- Partition data using hash or range strategies for predictable performance  
- Minimize shuffles by ordering joins properly and using broadcast hints when appropriate  
- Use DataFrames/Datasets over RDDs for better optimization and readability  
- Set explicit memory and executor limits; monitor skew with Spark UI and ganglia

### 3. Fault Tolerance

- Ensure checkpointing for long-running jobs  
- Use retry mechanisms for transient failures (e.g., S3 reads, DB writes)  
- Maintain idempotency via job parameters and snapshot isolation

### 4. Performance Optimization

- Cache intermediate results selectively  
- Use `coalesce()` instead of `repartition()` for reducing partition count  
- Avoid wide transformations in high-cardinality keys  
- Use `.explain()` and job metrics to tune stages

### 5. Observability

- All jobs must log `job_id`, `user_id`, `run_context`, and `source_data_path`  
- Emit custom metrics (e.g., rows processed, error count, duration)  
- Integrate with CloudWatch or Prometheus exporters for dashboards and alerts

### 6. Security & Data Compliance

- Use role-based access to EMR/Spark clusters  
- Encrypt data at rest (S3 with SSE-KMS) and in transit  
- Avoid writing PII to logs; use redaction libraries in Spark logging hooks

---

## Section 2: Apache Flink Principles

### 1. Use Cases for Flink

- Preferred for:
  - Real-time event stream processing (e.g., audit triggers, operational metrics)  
  - Stateful transformations on Kafka/EventBridge streams  
  - Complex windowing and out-of-order event processing  
  - Reconciliation pipelines with enriched data

### 2. State Management

- Use RocksDB or embedded state backends with periodic checkpoints  
- Enable exactly-once processing where latency permits  
- Externalize and monitor state size to avoid backpressure or disk overflow

### 3. Windowing & Event Time

- Use event time over processing time for business accuracy  
- Support tumbling, sliding, session windows as per domain SLAs  
- Handle late data using watermarks and side outputs

### 4. Fault Tolerance & Restarts

- Configure checkpoint intervals, restart strategies, and backoff times  
- Monitor job lifecycle via Flink Dashboard or CLI  
- DLQ all corrupted events for forensic replay

### 5. Deployment & Scaling

- Use Kubernetes-native Flink operator or EMR-on-EKS  
- Enable dynamic scaling and auto-recovery for resilient pipelines  
- Separate job managers and task slots for isolating workloads

### 6. Monitoring & Observability

- Tag streams with job metadata and domain-specific trace IDs  
- Publish metrics: throughput, lag, dropped records, checkpoint duration  
- Route logs to CloudWatch or custom observability pipelines

---

## Section 3: Amazon EMR Principles

### 1. Workload Targeting

- Use EMR for:
  - Spark and Hive batch workloads  
  - PySpark/Scala-based ML workflows  
  - Distributed ingestion from lake to warehouse  
  - Periodic feature pipelines for LLM models

### 2. Cluster Types & Cost Control

- Prefer EMR Serverless for ad hoc workloads  
- Use EMR on EKS for containerized, multi-tenant environments  
- Apply EC2 Spot instances where batch retries are acceptable  
- Tag clusters with cost-center, purpose, and TTL

### 3. Job Management

- Use Step Functions or Airflow to orchestrate EMR jobs  
- Separate compute for dev, QA, and prod  
- Track job lineage via CloudTrail, Data Catalog, and EMR logs

### 4. Data Access & Security

- VPC endpoints only; no open internet access  
- Encrypt data at rest with KMS; enable audit logging for S3 access  
- Use Lake Formation or IAM roles for fine-grained table access

### 5. Operational Hygiene

- Auto-terminate idle clusters  
- Limit max cluster duration via policies  
- Store all cluster logs in central S3 location for audit and troubleshooting

### 6. Monitoring & Automation

- CloudWatch dashboards for job duration, success rate, instance utilization  
- Alarms for cost spikes, task failures, long job durations  
- Integrate FinOps alerts into Slack/email with context (e.g., job name, reason)

---

## Summary

| Technology      | Batch vs Stream | Best For                                 | Not Ideal For                               |
|----------------|------------------|-------------------------------------------|---------------------------------------------|
| Apache Spark   | Batch            | ML training, lake ETL, joins, batch AI prep| Real-time event ingestion                   |
| Apache Flink   | Stream           | Event streams, stateful joins, anomaly detection | Large static batch jobs                  |
| Amazon EMR     | Flexible         | Cost-effective Spark/Hive/Presto on AWS   | Long-running ungoverned clusters            |

