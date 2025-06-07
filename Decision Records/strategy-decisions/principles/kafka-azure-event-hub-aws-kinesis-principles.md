# Kafka, Azure Event Hub, and AWS Kinesis Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Semi-annually or upon major updates to streaming architecture or cloud-native integrations

## Audience

This document is intended for:

- Platform and Event Architects  
- Backend and Streaming Engineers  
- DevOps and Cloud Engineers  
- AI/ML Engineers consuming real-time events

## Governance

- All event streaming infrastructure must be compliant with audit, latency, and cost expectations  
- Usage of Kafka, Event Hub, or Kinesis must be reviewed based on latency SLA, data volume, cross-cloud needs, and ecosystem maturity  
- Event contracts must be versioned and enforced regardless of the broker used

## Automation and Tooling Enablers

- **Schema Registry & Contract Validators**  
- **DLQ (Dead Letter Queue) Monitoring Dashboards**  
- **Cross-Cloud IAM Role Integration (e.g., AAD ↔ AWS IAM)**  
- **Consumer Lag & Throughput Dashboards**  
- **Event Replay Pipelines and TTL Cleanup Jobs**

---

## Section 1: Kafka Principles

### 1. Ideal Use Cases

- Internal high-throughput, low-latency message buses  
- Complex event processing with partitioned consumers  
- Streaming to data lakes and operational enrichment for AI workflows

### 2. Cluster Design & Topics

- Separate clusters for prod, non-prod, and sandbox  
- Use clear topic naming (`domain.event.version`)  
- Partition by meaningful keys (e.g., `desk_id`, `region`) for workload distribution

### 3. Delivery Semantics

- Support at-least-once or exactly-once semantics using Kafka Connect and transactional APIs  
- DLQ all poison messages and log deserialization errors

### 4. Observability

- Monitor consumer lag, error rates, and throughput per topic  
- Include `trace_id`, `event_type`, and `partition_id` in logs

### 5. Security

- Use TLS for encryption in transit  
- Enable Kerberos or mTLS for auth; no anonymous consumers  
- Configure ACLs per topic and role

---

## Section 2: Azure Event Hub Principles

### 1. Ideal Use Cases

- Streaming integration for Microsoft ecosystem services (Power BI, Logic Apps)  
- Telemetry ingestion from Azure-based services  
- Mid-volume use cases requiring enterprise IAM integration via Azure AD

### 2. Architecture and Throughput Units

- Assign event hubs by domain with consumer groups for each microservice  
- Allocate throughput units (TUs) based on expected ingestion rate  
- Use Event Hub Capture to store raw events in Blob Storage for traceability

### 3. Integration

- Native integration with Azure Stream Analytics, Synapse, and Data Factory  
- Federated access via AAD roles for producers and consumers  
- Compatible with Kafka protocol (Kafka surface for migration support)

### 4. Resilience & Monitoring

- Use Azure Monitor for ingestion rates, throttling, and latency  
- Route malformed or unauthorized events to a separate quarantine hub  
- Audit delivery failures per consumer group

### 5. Security & Compliance

- Enforce encryption at rest and in transit  
- Apply IP firewall rules and private endpoints  
- Use managed identities over shared access keys

---

## Section 3: AWS Kinesis Principles

### 1. Ideal Use Cases

- AWS-native streaming for event ingestion and downstream analytics  
- Real-time pipeline for exception alerts, LLM orchestration triggers, and forecasting anomalies  
- Seamless integration with AWS Lambda, Glue, Firehose, and Redshift

### 2. Stream Design

- Use Kinesis Data Streams for low-latency needs (millisecond to sub-second)  
- Separate Firehose delivery streams for archiving to S3 or loading into Redshift  
- Apply shard-level scaling based on throughput and parallelism

### 3. Observability

- Track put/get metrics, iterator age, and shard utilization  
- Emit custom metrics for consumer lag and backpressure alerts  
- Log stream access and anomalies to CloudWatch and S3

### 4. Delivery Semantics

- Default: at-least-once delivery  
- Use record deduplication downstream where idempotency is needed  
- Monitor retry counts and track failed delivery events in DLQs

### 5. Security and IAM

- Use fine-grained IAM roles per producer/consumer  
- Encrypt streams using KMS keys  
- Restrict network access via VPC endpoints

---

## Summary Matrix

| Technology       | Best For                                        | Managed? | Latency | Ecosystem Fit      | Not Ideal For                           |
|------------------|--------------------------------------------------|----------|---------|---------------------|------------------------------------------|
| Kafka            | High-throughput, cross-platform internal streams | ❌ Self/Cloud | Low     | Open Source & Hybrid | Overhead for small scale or edge cases   |
| Azure Event Hub  | Microsoft-native telemetry and enterprise events | ✅ Yes     | Medium  | Azure Ecosystem     | Open-source ecosystem, high-volume Kafka |
| AWS Kinesis      | Serverless AWS-native streaming + analytics      | ✅ Yes     | Low     | AWS Ecosystem       | Cross-cloud interoperability             |
