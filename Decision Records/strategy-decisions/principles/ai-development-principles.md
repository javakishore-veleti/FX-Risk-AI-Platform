# AI Development Principles and Practices

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform
- **Review Cadence**: Semi-annual (every 6 months) or after major AI capability introduction

## Audience

This document is intended for the following stakeholders:

- AI/ML Engineers
- Platform Engineers
- Prompt Engineers
- Product Managers designing AI workflows
- Compliance Officers evaluating AI touchpoints

## Governance

- **CIO-level mandates** from GDR-001 enforce AI explainability and traceability
- All new LLM-influenced features must be reviewed for compliance with these principles as part of the solution review process
- Violations or gaps must be tracked in technical debt registers and mitigated within 1–2 sprints

## Automation and Tooling Enablers

To ensure principles are adopted in practice:

- **Prompt Versioning**: All prompts must be stored in Git with unique SHA hashes and metadata
- **Prompt Testing Harness**: CI pipeline should include prompt eval suites using synthetic and real cases
- **Input Sanitization**: Implement guards in shared orchestration services to validate and cleanse inputs
- **Trace Logging**: Use a centralized logger that enforces input/response/model-version trace capture
- **Feedback Capture**: Integrate thumbs-up/down or tagging in the UI and forward to a feedback API
- **Compliance Hooks**: Periodically scan logs for incomplete trace data or missing explainability fields

---

## Principle Set

The following principles define the core expectations for LLM development across the platform:

### 1. Explainability by Design

- All LLM-driven workflows must expose their decision rationale to users.
- Explanations should accompany outcomes when shown to human analysts.
- Prompt-response pairs must be logged with contextual metadata.

### 2. Prompt Versioning and Safety

- Prompts must be version-controlled and stored with an immutable hash.
- Inputs must be validated and sanitized to avoid prompt injection.
- Use reusable prompt templates with parameter injection for consistency.

### 3. Evaluation and Benchmarking

- Each LLM-based capability must have defined acceptance tests and metrics.
- Evaluation harnesses must include human-in-the-loop and automated scoring.
- Periodic re-evaluation should be performed as model performance may drift.

### 4. Guardrails and Content Filtering

- Responses from LLMs must be filtered for profanity, hallucinations, and unsupported outputs.
- Implement max token limits and temperature constraints to reduce variance.
- Configure domain-specific vocabularies where applicable.

### 5. Logging and Traceability

- All LLM interactions must include:
  - Input prompt
  - LLM model version
  - Response text
  - Associated trace ID
  - User or service context
- Logs should be stored in AWS OpenSearch and/or S3 for downstream audits.

### 6. Feedback Loops and Continuous Improvement

- End users must be able to flag inaccurate or problematic responses.
- Feedback should be captured and routed to model tuning or fine-tuning workflows.
- Use this signal to improve prompt design and system reliability.

### 7. Regulatory Compliance

- Ensure LLM-generated actions are compliant with audit and regulatory guidelines.
- Avoid using LLMs for final decisions without human override unless explicitly reviewed.
- Maintain data residency, masking, and PII controls throughout the pipeline.

### 8. Performance and Cost Considerations

- Avoid overuse of expensive LLMs for tasks solvable with rules or classification.
- Establish service-level constraints (timeout, size limits, retries).
- Monitor token usage and cost per task to ensure efficiency.

### 9. Role Separation

- Clearly delineate which teams own:
  - Prompt templates
  - Model evaluation and tuning
  - Output routing and usage logging
- This ensures accountability and consistent governance across teams.

### 10. Ethics and Fairness

- LLM outputs must be tested for biased or discriminatory behavior.
- Datasets and tuning efforts should be inclusive and representative.
- Avoid reinforcing historical biases in operational or risk classification tasks.
