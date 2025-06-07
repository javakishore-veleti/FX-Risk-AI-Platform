# Retrieval-Augmented Generation (RAG) Stack Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Quarterly or upon onboarding new use cases with LLM + document grounding requirements

## Audience

- AI Engineers and LLM Integrators  
- Backend Engineers and Prompt Developers  
- Search Architects and Knowledge Management Teams

## Purpose

Define principles for building, scaling, and governing RAG-based LLM applications that ground responses in enterprise documents (e.g., audit logs, FX workflows, policy PDFs).

---

## Principle Set

### 1. Separation of Retrieval and Generation Layers

- Keep vector search, document chunking, and prompt orchestration modular  
- Retrieval logic should be independently testable and observable

### 2. Embedding Consistency

- Standardize embedding models (e.g., Azure OpenAI, Cohere) across domains  
- Version embedding models and store with chunk metadata for traceability

### 3. Chunking and Indexing Strategy

- Apply hierarchical chunking: by paragraph, section, or semantic boundary  
- Tag each chunk with `source_uri`, `timestamp`, `document_type`, `team`

### 4. Grounding Transparency

- Show retrieved sources in the UI  
- Include `confidence_score` and matched snippet location in logs

### 5. Prompt Architecture

- Use clear delimiters (`<<<docs>>>`, `<<<query>>>`)  
- Template prompts based on role and purpose: classification, Q&A, summarization, etc.  
- Track prompt versions and test coverage

### 6. Evaluation and Ranking

- Run regular evals on:
  - Faithfulness (hallucination rate)  
  - Answer completeness and formatting  
  - Grounding precision/recall  
- Use human-in-the-loop (HITL) where needed

### 7. Compliance and Access Control

- Filter documents by `desk_id`, `user_role`, or clearance level  
- Prevent sensitive document types from entering context without explicit override

### 8. Observability and Telemetry

- Log every retrieval + generation pair with `trace_id`, response size, latency  
- Alert on grounding miss rate, hallucination spikes, and prompt failures

### 9. Cost Control

- Cache vector queries and generation outputs per session/query hash  
- Monitor token usage and retrieval breadth per use case

### 10. Fallback and Degradation Modes

- If no high-quality chunks retrieved, return “no answer”  
- If LLM unavailable, show links to source documents directly
