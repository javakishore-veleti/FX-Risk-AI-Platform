# FX-Risk-AI-Platform
The umbrella meta-repo containing high-level documentation, architecture diagrams (TOGAF, BDAT, Zachman), and links to all the above component repos.

## Tooling Strategy 

This section outlines the chosen frameworks and tools for each repository in the `fx-risk-ai` platform, based on architecture fit, scalability, and domain needs.

| Repository Name                   | Framework / Tool     | Justification |
|----------------------------------|----------------------|---------------|
| **fx-risk-ai-portal**            | Angular              | Enterprise-grade UI framework with modular architecture and built-in routing; ideal for desk-level dashboards and ops workflows. |
| **fx-risk-ai-orchestration-api** | Django + Django REST Framework (DRF) | Excellent for structured APIs, backend workflows, admin interface, and ORM-based trade/exception models. |
| **fx-risk-ai-insights-engine**   | FastAPI              | Lightweight and high-performance framework for AI/LLM microservices (e.g., summarization, classification, embeddings). |
| **fx-risk-ai-forecasting-pipeline** | PySpark / AWS Glue | Suited for large-scale ETL and batch forecasting jobs (e.g., NOP prediction); integrates well with Amazon Forecast and S3. |
| **fx-risk-ai-validation-suite**  | Django + Pytest      | Ideal for test harnessing, integration test orchestration, and result visualization if needed. |
| **fx-risk-ai-ops-scripts**       | Django Management Commands | Cleanly integrates with Django projects; supports administrative tasks like loaders, migrations, and CLI-based scripts. |
| **fx-risk-ai-shared-libs**       | Python Package (Framework-Agnostic) | Prompts, embedding utilities, constants, and shared routing rules usable across all services without tight coupling. |
| **fx-risk-ai-infra**             | AWS CDK or Terraform | Infrastructure-as-code for provisioning AWS services (e.g., Bedrock, SageMaker, Glue, S3, IAM, QuickSight). |
| **fx-risk-ai-platform**          | Markdown + Diagrams + Docs | Meta-repo for linking components, managing strategy artifacts, BDAT/Zachman diagrams, and system-wide documentation. |

