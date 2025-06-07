# AWS Bedrock Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Quarterly or after adoption of new foundation models or prompt orchestration patterns

## Audience

- AI/ML Engineers  
- Backend Developers integrating LLMs via Bedrock  
- Platform and Security Architects  
- Compliance and Audit Stakeholders

## Purpose

This document defines how Amazon Bedrock should be adopted across the Fx-Risk-AI Platform—covering LLM usage, prompt control, traceability, security, and cost governance.

---

## Principle Set

### 1. Model Selection Must Be Use-Case Driven

- Use Claude, Titan, or Cohere models based on:
  - Output structure needs (e.g., classification, summarization, generation)  
  - Prompt length and token limits  
  - Cost-performance tradeoffs  
- Each use case must document the selected model, version, and evaluation rationale

### 2. Prompt Engineering as Code

- All prompts must be version-controlled in Git  
- Use parameterized templates, not inline strings  
- Avoid business logic embedded in prompts—separate formatting from intent

### 3. Traceability of All LLM Calls

- Every Bedrock request must log:
  - Prompt input and type  
  - Model name and version  
  - Inference timestamp  
  - `user_id`, `desk_id`, and `trace_id`  
  - Response token count and latency  
- Logs must be centralized to OpenSearch or CloudWatch

### 4. Access Control and Security

- Use IAM roles for Bedrock access—never API keys in code  
- Scope roles to only permitted models and environments  
- Block non-prod roles from using production prompts or sensitive data

### 5. Prompt Evaluation and Safety

- Each prompt must be tested against:
  - Hallucination scenarios  
  - Prompt injection  
  - Sensitive output leakage  
- Include assert tests and eval harnesses in CI/CD workflows

### 6. Feedback and Fine-Tuning Loop

- Allow users to rate LLM output or flag unclear/incomplete results  
- Route feedback to a prompt refinement or RAG pipeline  
- Capture manual overrides and corrections for future iterations

### 7. Cost Governance

- Enable Bedrock usage logging and tagging by team, service, and prompt  
- Set monthly token budgets per desk or workload  
- Trigger alerts for spikes in invocation volume or average token usage

### 8. Multi-Model and Multi-Vendor Readiness

- Architect prompt orchestration to support switching models (e.g., Bedrock → OpenAI)  
- Avoid vendor lock-in by abstracting LLM invocations behind service interfaces

### 9. Guardrails and Output Moderation

- Enable built-in content filters where supported (e.g., Amazon Titan moderation)  
- Post-process outputs using regex filters or rule-based validators  
- Do not expose raw LLM output to end users without safety wrapping

### 10. Production Readiness Criteria

- All Bedrock-based features must pass:
  - Trace logging test  
  - Prompt auditability  
  - Token cost predictability  
  - Fallback scenario (timeout, no response, overlimit)  
- Approval from AI Platform Engineering team is required before go-live
