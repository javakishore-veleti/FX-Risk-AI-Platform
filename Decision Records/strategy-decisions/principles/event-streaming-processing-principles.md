# Event Streaming & Processing Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Quarterly or after updates to event models, broker configurations, or processing pipelines

## Audience

This document is intended for:

- Backend Developers and Event System Designers  
- Data Engineers and Stream Processing Teams  
- DevOps and Observability Engineers  
- Architects responsible for real-time and asynchronous workflow design

## Governance

- All event-driven systems must follow platform-approved messaging protocols, schemas, and retry policies  
- Services producing or consuming events must register contracts and SLAs  
- Event design and evolution must be reviewed through ADRs and validated in CI

## Automation and Tooling Enablers

- **Schema Registry**: Validates message formats (e.g., Avro, JSON Schema)  
- **Event Catalog**: Central repository for event names, versions, producers, and consumers  
- **Replay and DLQ Tools**: Enable controlled reprocessing and failure diagnostics  
- **CI Matchers**: Contract testing to prevent producer-consumer drift

---

## Principle Set

### 1. Event-First Architecture

- Design workflows around event triggers, not polling or batch processes  
- Define event schemas before implementing logic or storage changes

### 2. Strong Typing and Versioned Contracts

- Events must follow an explicit schema: required fields, types, enums, and validation rules  
- Contracts must include `eventType`, `eventVersion`, `sourceService`, `traceId`, and `timestamp`  
- Use additive evolution (e.g., append-only schema) and support parallel versioning where needed

### 3. At-Least-Once Delivery and Idempotency

- All consumers must implement idempotent processing logic  
- Message identifiers must be used to detect duplicates and correlate retries

### 4. Fault Tolerance and Retry Safety

- Failed messages must go to a Dead Letter Queue (DLQ) with full context and error metadata  
- Retry logic must use exponential backoff and jitter  
- Messages must be preserved until explicitly purged or expired per retention policy

### 5. Backpressure and Rate Awareness

- Use streaming platforms (e.g., Kafka, Kinesis, EventBridge) that support consumer lag metrics  
- Consumers must track offsets and handle surge gracefully without data loss  
- Configure auto-scaling for processing components

### 6. Event Traceability

- Events must propagate a `traceId` to correlate logs, metrics, and spans  
- Logging context must include topic, partition, offset, and source component

### 7. Consumer Decoupling

- Use fan-out models like SNS, Kafka topics, or EventBridge for multi-consumer scenarios  
- Avoid tightly coupling business logic to a single topic or event processor

### 8. Stream-Table Hybrid Processing

- Use stream enrichment with external lookups sparingly and asynchronously  
- For stateful joins or aggregations, use frameworks like Apache Flink, Kafka Streams, or AWS Glue Streaming

### 9. Replayability and Audit

- Events must be persisted with sufficient retention for downstream replay  
- Consumers must support replay for specific partitions, timestamps, or offsets  
- Audit logs must track all event ingestion, enrichment, and transformation steps

### 10. Observability and Monitoring

- Alert on consumer lag, DLQ volumes, throughput drops, and schema mismatches  
- Dashboards must include real-time metrics per topic, producer, and consumer  
- All anomalies must be traceable to a specific trace ID, team, and component
