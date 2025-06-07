# Azure AI Search Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Annually or upon introduction of new semantic search-based workloads

## Audience

- AI/ML Engineers and Retrieval Developers  
- Knowledge Management Architects  
- Azure Platform Engineers and Data Engineers  
- LLM Integration and Compliance Teams

## Purpose

This document establishes best practices for securely using **Azure AI Search** as a vector-aware, semantic-aware retrieval layer for Retrieval-Augmented Generation (RAG), enterprise search, and LLM context enrichment across hybrid cloud and Microsoft-native workloads.

---

## Principle Set

### 1. Search Use Cases Must Be Goal-Aligned

- Use Azure AI Search for:
  - LLM prompt grounding in RAG scenarios  
  - Business document Q&A (e.g., policies, procedures)  
  - Semantic access to knowledge bases or contracts  
- Avoid using it for full data warehousing, unbounded metrics queries, or log analytics

### 2. Semantic and Vector Indexing Strategy

- Index both keyword and vector embeddings for hybrid retrieval  
- Store embedding metadata (e.g., `source_uri`, `document_type`, `timestamp`)  
- Use Azure OpenAI, HuggingFace, or Cohere models for vector generation—document embedding model and version

### 3. Index Design and Naming

- Use consistent naming: `fx-risk-{domain}-semantic-index`  
- Separate indexes by business capability (e.g., `forecasting`, `audit-trace`, `policy-docs`)  
- Apply filters by `desk_id`, `role`, `classification`, and `language` in search pipelines

### 4. Prompt Integration and Grounding

- Use top-k retrieval to fetch relevant documents/snippets before LLM invocation  
- Ensure grounding content is appended with clear separators in prompt context  
- Track which retrieved documents contributed to which generated answers (`attribution`)

### 5. Relevance and Ranking Evaluation

- Evaluate precision, recall, and ranking order using labeled datasets  
- Implement feedback capture to measure helpfulness, accuracy, and completeness  
- Use similarity scores and embedding distance for threshold tuning

### 6. Security and Access Controls

- Restrict index creation and search access to approved roles only  
- Apply AAD-based authentication for developers and API access  
- Mask or redact PII from indexed content before upload

### 7. Content Ingestion & Enrichment

- All indexed data must include:
  - Title, body, tags, source, last updated timestamp  
  - Chunked version (e.g., by paragraph, page, or section)  
- Enrich content using cognitive skills (e.g., key phrases, categories, language detection)

### 8. Monitoring and Observability

- Log all queries with `user_id`, `desk_id`, `search_id`, `latency`, and result count  
- Create dashboards for query trends, null responses, query types, and spike alerts  
- Alert on indexing failures, quota exhaustion, and high latency

### 9. Cost and Capacity Management

- Monitor API calls, index size, and query volume  
- Use standard tier unless latency or scale demands dedicated tier  
- Auto-purge outdated or unused content with TTL policies

### 10. Auditability and Compliance

- Indexes used in RAG flows must log:
  - Retrieval trace  
  - Search version  
  - Matching content excerpt  
  - Prompt output trace ID  
- Align with AI explainability and audit policy—especially for regulated content domains
