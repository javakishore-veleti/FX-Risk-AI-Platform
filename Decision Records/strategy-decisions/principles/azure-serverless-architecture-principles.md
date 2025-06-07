# Azure Serverless Architecture Principles

**Owner**: Chief Architect  
**Audience**: Azure Cloud Engineers, Architects, DevOps, Compliance Teams  
**Governance**: Azure Architecture Review Board (ARB)  
**Automation**: Enforced via Azure Policy, Bicep Templates, and GitHub Actions

## Purpose

This document defines architectural principles for leveraging Microsoft Azure's serverless platform. It ensures teams build resilient, scalable, secure, and auditable solutions aligned with enterprise and regulatory mandates.

---

## 1. Prefer Azure Native Serverless Services

- Use Azure Functions for compute, Event Grid for event routing, and Logic Apps for workflow orchestration.  
- Prefer Azure API Management (APIM) over self-hosted proxies or gateways.  
- Utilize Cosmos DB Serverless, Azure Storage, or Azure SQL Hyperscale where appropriate.

---

## 2. Design Stateless, Event-Driven Functions

- Each Azure Function should perform a discrete unit of work.  
- Favor queue-based triggers (Storage Queue, Service Bus) for decoupling.  
- Use Durable Functions only when orchestration patterns require it.

---

## 3. Orchestration via Logic Apps or Durable Functions

- Logic Apps for declarative, connector-heavy workflows.  
- Durable Functions for complex patterns (e.g., fan-out/fan-in, human interaction, long running).  
- Apply retry, compensation, and timeout strategies.

---

## 4. Use Event Grid for Reactive Designs

- Connect multiple Azure services through Event Grid for loosely coupled pub/sub patterns.  
- Use custom topics to support domain-level boundaries and schema evolution.  
- Embrace event contracts and schema validation.

---

## 5. Built-in Observability and Telemetry

- Use Application Insights for all serverless compute.  
- Correlate telemetry across distributed workflows.  
- Collect custom metrics and structured logs with log analytics integration.

---

## 6. Identity and Access Management

- Leverage Managed Identities over hardcoded credentials.  
- Assign least-privilege access via Azure Role-Based Access Control (RBAC).  
- Define scopes and usage boundaries through Azure AD app registrations and scopes.

---

## 7. Infrastructure as Code (IaC)

- All serverless deployments must use Bicep, Terraform, or ARM templates.  
- Include CI checks for security compliance and resource policy validation.  
- Use version-controlled service catalogs for repeatable deployments.

---

## 8. Security and Data Protection

- Use Azure Key Vault for secrets, credentials, and keys.  
- Encrypt data in transit and at rest using customer-managed keys (CMK) where required.  
- Apply input/output sanitization and schema enforcement on all APIs.

---

## 9. Cost Optimization and Scaling

- Configure autoscale for Azure Functions and queues based on metrics.  
- Monitor invocation cost and cold start impact.  
- Use Premium or Dedicated plans only when startup latency or VNET access is critical.

---

## 10. Lifecycle Governance and Compliance

- Assign resource tags for ownership, environment, data sensitivity.  
- Apply lifecycle policies for versioning, retention, and archiving.  
- Include serverless assets in risk assessments and architecture reviews.

---

## Tags

`#azure-serverless` `#functions` `#logic-apps` `#event-grid` `#serverless-security` `#cost-governance` `#observability` `#infra-as-code`
