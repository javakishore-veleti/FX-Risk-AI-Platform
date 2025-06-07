# Architecture Review Method — Fx-Risk-AI Platform

## Ownership

- **Owner**: Architecture Review Board (ARB), led by Chief Architect
- **Review Cadence**: Per major change (new module, integration, AI workflow, infra shift)

## Audience

- Platform and Domain Architects
- Engineering Managers and Tech Leads
- Security, Compliance, and DevOps Representatives

## Purpose

To provide a lightweight, repeatable, and high-trust method of ensuring solutions align with strategic architectural principles, platform strategy, and delivery guardrails — without blocking execution velocity.

---

## Review Types

### 1. Informal Consult

- Used during PoC, spike, or early design
- Peer-to-peer session with architect, with guidance and principle mapping

### 2. Lightweight Review

- Required before MVP implementation
- Covers BDAT alignment, API boundaries, logging, security, cost, and extensibility
- Uses a shared checklist, reviewed asynchronously

### 3. Formal Review

- For cross-domain integrations, AI workflows, shared components, or cloud boundary shifts
- Includes Architecture Scorecard, System Diagram, and Draft ADR(s)
- ARB sign-off required for release approval

---

## Review Checklist Categories

- Business Alignment and Capability Mapping
- Domain Modularity and Reuse
- Security and Compliance (IAM, secrets, audit)
- Observability (logs, metrics, traces, dashboards)
- AI Prompting and Model Traceability (if applicable)
- Data Flows and Ownership Boundaries
- CI/CD Pipelines and Deployment Policies
- Cost Optimization and Resilience

---

## Tooling and Traceability

- Reviews stored in Git or Notion with links to diagrams, ADRs, and decision outcomes
- Scores tracked in Architecture Dashboard
- Checklist items linked to `architecture-principles.md`

---

## Summary

This method ensures:

- Speed with structure
- Governance with autonomy
- Strategic architecture with tactical agility
