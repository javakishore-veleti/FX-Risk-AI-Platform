# AWS Serverless Architecture Principles

**Owner**: Chief Architect  
**Audience**: Cloud Engineers, Solution Architects, DevOps, Security Teams  
**Governance**: Cloud Architecture Review Board (CARB)  
**Automation**: Enforced via AWS Config Rules, CI/CD checks, and IaC linter pipelines

## Purpose

This document outlines the strategic principles and best practices for designing and deploying applications using AWS-native serverless services. It aims to ensure scalability, resilience, operational efficiency, and cost-effectiveness while maintaining compliance and governance.

---

## 1. Service-First Thinking

Prefer native AWS services over custom-built components:
- Use AWS Step Functions for orchestration over custom state machines.
- Favor Amazon EventBridge for loosely coupled event routing.
- Rely on Amazon DynamoDB, S3, or Aurora Serverless for data persistence.

---

## 2. Granular and Stateless Functions

- Design Lambda functions to be stateless and single-responsibility.
- Avoid monolithic Lambdas that violate separation of concerns.
- Store execution state externally (e.g., DynamoDB, SQS, S3).

---

## 3. Event-Driven by Default

- Adopt EventBridge, SQS, SNS, and DynamoDB Streams for triggering workflows.
- Use Lambda destinations or Step Functions for handling success/failure paths.
- Design for asynchronous decoupling and retry semantics.

---

## 4. Cold Start Minimization

- Use provisioned concurrency for latency-sensitive Lambdas.
- Choose runtimes with low cold-start overhead (e.g., Node.js, Python).
- Group latency-tolerant services separately to avoid over-engineering.

---

## 5. Observability and Tracing Built-In

- Mandate usage of AWS X-Ray, CloudWatch Logs, and structured logging.
- Annotate traces with business metadata (e.g., transaction ID, user ID).
- Enable end-to-end correlation across event chains.

---

## 6. IAM and Least Privilege

- Use IAM roles and policies scoped to minimum required actions.
- Enforce function-level identity separation.
- Use resource-based policies on S3, SQS, Lambda when appropriate.

---

## 7. Infrastructure as Code (IaC)

- All serverless components (Lambdas, APIs, permissions) must be provisioned via IaC (e.g., AWS CDK, SAM, or Terraform).
- Repositories must include CI validation and drift detection.

---

## 8. Secure Inputs and Outputs

- Validate input payloads using schema validation or API Gateway models.
- Enforce encryption at rest and in transit using KMS, TLS, and managed keys.
- Sanitize logs to avoid leaking PII or secrets.

---

## 9. Cost and Quota Awareness

- Use cost allocation tags and budgets on serverless resources.
- Track request volume, concurrency, and quota consumption.
- Prefer short-lived functions and SQS batching where applicable.

---

## 10. Lifecycle and Governance

- All serverless services go through architecture review.
- Versioning and lifecycle policies must be applied to Lambda, API Gateway stages, etc.
- Routinely deprecate and archive unused services or stale event topics.

---

## Tags

`#aws-serverless` `#architecture-principles` `#lambda` `#step-functions` `#eventbridge` `#security` `#governance` `#cost-optimization`
