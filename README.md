# FX-Risk-AI-Platform

## Purpose of this Repo
The umbrella meta-repo containing high-level documentation, architecture diagrams (TOGAF, BDAT, Zachman), and links to all the above component repos.

## Introduction

The **Fx-Risk-AI-Platform** is an enterprise-grade, modular AI architecture designed to transform operational efficiency, exception handling, and compliance workflows across Forex (FX) desk operations. It leverages generative AI, machine learning, orchestration APIs, and intelligent automation to deliver real-time, desk-aligned insights and actions.

> This meta-repo links all sub-repos: UI, orchestration, AI services, forecasting pipelines, infrastructure, and more.

---

## Strategic Objectives

- **Reduce Operational Friction:** Automate FX exception classification, ticket routing, and chat summarization across desks and shifts.
- **Enable Real-Time Desk Forecasting:** Predict NOP volumes and SLA risks for day-end planning and resourcing.
- **Streamline Trade Confirmations & Routing:** Intelligent confirmation handoffs to the right desk, region, or counterparty.
- **Democratize AI Without Deep ML:** Leverage zero-shot and prompt-based AI tools to unlock value without complex model training.
- **Strengthen Audit & Compliance:** Flag trade amendments, late breaks, and inconsistencies with hybrid rule-based + LLM logic.

---

## Chief Architect Perspective

> _"The job is not to deploy AI, but to **elevate operational clarity** across time zones, currencies, and exception categories."_  
> — Chief Architect

### Strategic Mindset

- **Business-Driven Design:** Architecture must **map directly to business outcomes** — NOP forecasting, exception speed, desk-level clarity — not abstract tech outputs.
- **"It Works" vs. "It Transforms":** Deliver **step-change operational value**, not just minimal integrations. Think in terms of FX desk enablement, not API coverage.
- **Cloud-Native First:** Default to **AWS-native primitives** (Bedrock, SageMaker, Glue, Lambda) to maximize scalability, observability, and integration velocity.
- **AI as a Co-Pilot, Not a Toy:** Integrate generative AI where it reduces ambiguity, resolves decisions, or eliminates queue fatigue — not just for demo appeal.
- **Platform Thinking, Not Point Projects:** The platform is a **fabric of intelligent services**, not a set of siloed bots or dashboards.

### Architecture Strategy

- **Modular Microservices:** Each repo follows domain-driven design — from LLM insights to forecasting pipelines and orchestration layers.
- **Composable Infrastructure:** Built with AWS-native primitives (S3, Lambda, SageMaker, Bedrock, Glue) using IaC principles.
- **Validation-First Mindset:** Each AI outcome is testable, traceable, and auditable across environments.
- **Prompt Reuse & Standardization:** Shared prompt libraries ensure consistency and reduce redundancy in AI logic.
- **Pluggable AI Agents:** Each Bedrock/LLM component is deployable independently and callable from orchestration APIs.

---

## CIO-Level Value Proposition

> _"We don’t just automate workflows — we create real-time, insight-rich FX operations that scale."_  
> — CIO

### Strategic Impact Areas

| Value Area                | Business Impact |
|---------------------------|-----------------|
| **Operational Predictability** | Desk-level visibility into NOP volume surges, SLA breach risks, and margin reconciliation complexity. |
| **AI-Augmented Ops**         | LLM-based exception classifiers, summarizers, and next-best-action models reduce manual load across desks. |
| **Cross-Team Coordination**  | Unified orchestration logic across Ops, Treasury, and Legal teams improves coverage and compliance. |
| **Built-in Audit & Governance** | Automated detection of late trades, duplicate disputes, and unresolved escalations — logged with context. |
| **Cloud-Native Scalability** | Built on Bedrock, SageMaker, Lambda, and QuickSight for high-performance, serverless, scalable ops. |
| **Faster Time to Value**     | Zero-shot and prompt-based AI use cases reduce development cycles from months to weeks. |

### CIO KPIs to Track

- Reduction in FX break resolution time
- SLA compliance rate across time zones
- Escalation handoff consistency and summary quality
- LLM action suggestion accuracy and desk feedback
- NOP forecast accuracy (vs actual desk volumes)
- Audit flag resolution rates

---

## Component Repositories

Refer to the [Tooling Strategy ](#️-tooling-strategy) for a full list of linked repositories and the frameworks used in each.

---

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


