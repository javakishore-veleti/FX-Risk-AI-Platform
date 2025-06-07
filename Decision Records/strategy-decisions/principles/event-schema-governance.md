# Event Schema Governance

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Quarterly or with each domain's new event type or schema evolution

## Audience

- Event producers and consumers  
- Data engineers, backend developers, integration architects  
- Platform governance teams

## Purpose

Ensure all event-based communication within the platform is version-controlled, discoverable, auditable, and evolution-friendly across services, teams, and clouds.

---

## Governance Model

### 1. Schema Registry Enforcement

- All events must be registered in a central schema registry (e.g., AWS Glue, Confluent Schema Registry, Azure Schema Registry)
- Contracts must define:
  - `event_type`
  - `event_version`
  - `source`
  - `trace_id`
  - `payload`
  - Optional: `metadata`, `context`, `errors`

### 2. Versioning Strategy

- Backward-compatible changes (additive fields) → New minor version  
- Breaking changes → New major version and topic suffix  
- Consumers must be allowed to subscribe to a specific version or a backward-compatible group

### 3. Ownership & Stewardship

- Every event type must have:
  - A product owner (business domain)  
  - A technical owner (engineering team)  
- Schema changes require approval from both roles

### 4. Validation and Testing

- Schema validation must run in CI for both producers and consumers  
- Producers must not publish invalid or undocumented event formats  
- Consumers must have fallback handling for version mismatches

### 5. Discoverability & Searchability

- All schemas must be tagged with:
  - `domain`, `purpose`, `sensitivity`, `team`, and `environment`
- An internal UI should support filtering and previewing fields per schema

### 6. Audit and Retention

- Every schema change must include:
  - Changelog  
  - Migration notes  
  - Impact assessment  
- Retain schema versions for at least 12 months

### 7. Cross-Cloud and Polyglot Support

- Schemas should be defined in a language-agnostic format (e.g., Avro, Protobuf, JSON Schema)  
- Support for serializers in Python, Java, and TypeScript must be maintained

### 8. Security & Data Classification

- Schemas must classify sensitive fields (e.g., PII, financial identifiers)  
- Use tags for downstream masking or redaction logic  
- Access control for producing/consuming sensitive schemas must be enforced by domain

