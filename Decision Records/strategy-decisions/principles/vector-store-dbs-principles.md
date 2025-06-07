# Vector Store DBs Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Quarterly or with new embeddings or RAG pipelines

## Audience

- AI Infrastructure Engineers  
- Semantic Search and RAG Developers  
- Data Governance and Security Architects

## Purpose

Provide guidance on using vector stores (e.g., Pinecone, OpenSearch, Qdrant, Weaviate, FAISS) to manage document embeddings, similarity search, and semantic recall with proper governance.

---

## Principle Set

### 1. Use Case Suitability

- Use vector stores for:
  - Semantic search  
  - Prompt grounding for LLMs  
  - Similarity detection across documents or tickets  
- Do NOT use for transactional or OLAP workloads

### 2. Index Design and Metadata Tagging

- Each vector must be stored with:
  - `embedding_model_id`, `document_uri`, `chunk_id`  
  - `domain`, `language`, `sensitivity_level`

### 3. Distance Metrics Must Be Explicit

- Define and document similarity metric per index (e.g., cosine, dot product, L2)  
- Use normalized vectors where appropriate

### 4. Embedding Lifecycle Management

- Record `embedding_version`, `created_at`, and `source_document_hash`  
- Recompute embeddings when model version or chunking logic changes

### 5. Access Control and Namespaces

- Use namespace per domain or team  
- Enforce IAM-based or token-based access to prevent cross-domain reads

### 6. Observability and Drift Detection

- Track top-k retrieval precision and recall  
- Monitor changes in average similarity scores over time

### 7. Performance and Cost Optimization

- Use filtering (metadata pre-selection) to reduce index scan volume  
- Compact or shard large indexes based on usage patterns  
- Archive rarely queried embeddings to cold storage

### 8. Schema Validation and Consistency

- Validate vector dimensions against embedding model  
- Ensure metadata fields conform to expected schema and enum sets

### 9. Fallback and Degradation Strategy

- If no vector match exceeds confidence threshold, trigger rules-based fallback  
- Expose retrieval misses and distance metrics in logs

### 10. Interoperability

- Use OpenAPI or gRPC interfaces for cross-platform calls  
- Adopt open formats (e.g., JSON, protobuf) for embedding payloads
