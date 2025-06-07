# AI-Specific Architecture Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform
- **Review Cadence**: Quarterly or upon introduction of new AI services or workflows

## Audience

This document is intended for:

- AI/ML Engineers
- Platform Architects
- Data Scientists
- Compliance and Governance Reviewers

## Governance

- Any new AI capability (LLM, classifier, forecaster) must map to these principles.
- Architectural diagrams and ADRs must indicate AI model boundaries, traceability, and fallback logic.
- Models must be independently evaluable and testable through a sandbox or staging pipeline.

## Automation and Tooling Enablers

- **Model Catalog**: Central registry of all models, owners, input schema, and trace coverage.
- **Prompt Registry**: Git-tracked templates, tagged with usage context and owner.
- **Eval Harness**: Automated scoring of AI behaviors using reference datasets.
- **Compliance Hooks**: Auto-checks for missing prompt versions, trace logs, masking, and explanations.

---

## Principle Set

### 1. AI Must Serve a Business Workflow

- Models should not be deployed without integration to an end-to-end use case.
- LLMs must improve productivity, reduce SLA breaches, or unlock new signals.

### 2. Prompts Are Code

- Prompt templates must be versioned and reviewed like application code.
- Inline prompt overrides or hardcoded strings are not allowed.

### 3. Human-in-the-Loop Where Needed

- Workflows must allow humans to review or override LLM suggestions.
- For high-risk domains, LLMs should only suggest—not execute—decisions.

### 4. Explainability and Traceability

- All predictions must include the prompt, model used, version, and context.
- Trace IDs must link across UI, service, prompt, and model output.

### 5. Reusable Evaluation Frameworks

- Models and prompts must be scored via reusable harnesses and test scenarios.
- Accuracy, completeness, hallucination, and bias must be measured continuously.

### 6. Progressive Exposure

- New prompts or models should be deployed gradually (A/B, desk-only, canary).
- Track error rates and user overrides to determine maturity.

### 7. Role-Based Access to AI Controls

- Prompt configuration and test overrides must be permission-controlled.
- Trace viewers and feedback interfaces must respect user roles and data security.

### 8. Degrade Gracefully

- All AI calls must include timeout, retry, and fallback logic.
- Fallback may be: “Ask an operator”, default rules, or previous answers.

### 9. Training Data Lineage and Bias Check

- All training datasets must have data provenance and PII checks.
- Re-training cycles must include fairness validation and audit readiness.

### 10. Model Usage Limits and Cost Controls

- Limit tokens or invocations per user, per desk, or per workload type.
- Monitor budgeted LLM usage vs. actual outcomes in QuickSight or CloudWatch.
