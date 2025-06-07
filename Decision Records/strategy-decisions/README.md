# Strategy-Level Decision Records

This folder captures CIO and CTO-level decisions made to guide the strategic, technological, and governance direction of the Fx-Risk-AI Platform. These records supplement the technical Architecture Decision Records (ADRs) and help ensure that business transformation, platform scale, and compliance needs are formally documented.

## Record Types

| Type | Owner | Description |
|------|-------|-------------|
| SIR  | CIO   | Strategic Initiative Records — document transformation initiatives aligned with business KPIs |
| TDR  | CTO   | Technology Direction Records — define platform-wide technology choices or direction |
| GDR  | CIO/CTO | Governance Decision Records — capture policies, audit requirements, and compliance mandates |

## Folder Contents

Each file follows a versioned and unique naming convention (e.g., `sir-001.md`, `tdr-001.md`). They include:

- Business or technology context
- Decision scope and stakeholders
- Rationale and intended outcomes
- Links to related ADRs or technical records

## How to Contribute

1. Choose the correct decision type (SIR, TDR, GDR)
2. Use a consistent filename (e.g., `sir-002-nop-forecasting.md`)
3. Use one of the markdown templates from `templates/` if available
4. Submit as a version-controlled pull request for review and approval
