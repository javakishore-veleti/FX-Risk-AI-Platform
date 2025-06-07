# Non-API Contracts Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Annually or upon any platform-wide changes in data integration or messaging design

## Audience

This document is intended for:

- Data Engineers and Platform Architects  
- Application Developers working with event-driven workflows  
- Domain Teams producing or consuming file-based or streaming data  
- QA and Governance Leads responsible for contract testing and schema enforcement

## Governance

- All non-API interfaces must follow these contract principles before release  
- Producers are responsible for publishing and versioning contract definitions  
- Consumers must build to documented schemas, not reverse-engineered structures

## Automation and Tooling Enablers

- **Schema Registry**: Avro/JSON schema repositories with version control  
- **Contract Testing**: Use tools like `pact`, `kafka-schema-registry`, or internal CI matchers  
- **Glue Catalog & S3 Policies**: Used to manage file schema and dataset lineage  
- **Contract Discovery Portals**: Registry of event types, file specs, and ownership metadata

---

## Principle Set

### 1. Contract-as-Product

- Every file format, event schema, or message structure is treated as a **versioned contract**  
- The producing team owns its lifecycle, changelog, and documentation

### 2. Schema-First Design

- All contracts must start with a formal schema (e.g., JSON Schema, Avro, CSV spec)  
- Define field types, required vs optional, enums, and nested objects explicitly

### 3. Backward Compatibility

- Contracts must be additive or evolve without breaking consumers  
- Deprecated fields should remain until all consumers migrate  
- Breaking changes require a new version (e.g., `forecast.v1.json` → `forecast.v2.json`)

### 4. Deterministic Formats

- Fields must be ordered and serialized predictably  
- Timestamps must be ISO 8601 with time zone; numbers must use consistent precision  
- Avoid dynamic field names or non-standard encodings

### 5. Discoverability and Ownership

- All non-API contracts must be registered in a shared catalog with:  
  - Schema or file spec  
  - Sample payload  
  - Producer team and point of contact  
  - Consumer teams  
  - Contract version and update history

### 6. Consumer Isolation

- Consumers must **not** parse undocumented fields or infer types  
- Consumer logic must validate inputs before processing (fail-safe, not fail-open)

### 7. Secure and Controlled Access

- File-based contracts must reside in protected S3 buckets with IAM-based policies  
- Queue and topic subscriptions must enforce access boundaries and encryption  
- Logs must capture `who`, `when`, and `why` of file and event usage

### 8. Observable Lineage

- Data derived from non-API contracts must be traceable to the source  
- Use tagging, event correlation IDs, and metadata fields (e.g., `source`, `schemaVersion`)

### 9. Testability and Mocking

- Contract consumers must support local testing with sample payloads  
- Shared mocks should be stored alongside schema definitions  
- Use CI pipelines to validate consumer logic against current schema version

### 10. Alignment with API Contracts

- When the same business object is exposed via API and event/file, schemas must be structurally aligned  
- Field names, types, and nesting should match unless documented deviations exist
