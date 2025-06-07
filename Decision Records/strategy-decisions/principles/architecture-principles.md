# Architecture Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform
- **Review Cadence**: Annual or aligned with major architectural refactoring cycles

## Audience

This document is intended for:

- Platform and Domain Architects
- Tech Leads
- Principal Engineers
- Architecture Governance Board Members
- Compliance and Risk Oversight Teams

## Governance

- All major design documents and ADRs must demonstrate alignment with these principles.
- Architecture reviews must assess solution conformity with this set before approval.
- Deviations require justification, risk assessment, and tracking in decision records.

## Automation and Tooling Enablers

- **Architecture Scorecard**: Solution templates must include principle adherence checklists.
- **CI Policy Hooks**: Enforce modular boundaries and interface validation where possible.
- **Service Classification**: Tagging of core/shared/edge services enables structural reviews.
- **Architecture Dashboard**: Visualize BDAT and repo alignment across services using metadata.

---

## Principle Set

### 1. Business Alignment First

- Architecture decisions must align to business value: faster FX ops, improved SLAs, auditability.
- Support line-of-business modularity (e.g., desks, operations, legal).

### 2. Modularity and Domain Separation

- Boundaries should follow business capabilities and ownership domains.
- Use separate repos or modules to enforce isolation (e.g., orchestration vs. insights).

### 3. Reusability Across Workflows

- Build APIs, prompts, and rules to be desk-agnostic where possible.
- Share orchestration flows and UI components in `fx-risk-ai-shared-libs`.

### 4. Cloud-Native, Serverless First

- Favor AWS-native serverless services (e.g., Lambda, Glue, Bedrock).
- Use containers only when workload characteristics demand it.

### 5. Secure by Default

- All services must require IAM or JWT-based access.
- No public endpoints unless explicitly approved.
- Encrypt all data in transit and at rest.

### 6. Observable and Measurable

- Emit logs, metrics, and traces in standard formats.
- Enable traceability from UI → Orchestration → API → Model → Logs.

### 7. Loose Coupling, Strong Contracts

- Interfaces between services must be versioned and decoupled.
- Changes must not require downstream rewrites or hard dependencies.

### 8. Auditability and Governance Built-In

- All actions and decisions should be logged with trace IDs.
- Data lineage, access controls, and prompt versions must be reviewable.

### 9. Extensibility via Configuration

- Features and flows should be toggleable/configurable, not hardcoded.
- Use config stores (e.g., Parameter Store, SSM) for runtime customization.

### 10. Minimal Viable Architecture (MVA)

- Design for fast PoC while preserving scale pathways.
- Don’t overbuild up front — favor adaptable building blocks.
