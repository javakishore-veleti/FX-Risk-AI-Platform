# Traditional / Classical Machine Learning (ML) Principles

## Ownership

- **Document Owner**: Chief Architect, Fx-Risk-AI Platform  
- **Review Cadence**: Annually or upon the introduction of new classical ML workloads or pipelines

## Audience

- ML Engineers and Data Scientists  
- Model Governance and Risk Teams  
- Feature Engineering and Forecasting Teams  
- Compliance, QA, and Audit Stakeholders

## Purpose

This document provides principles to ensure classical ML models are trained, deployed, and maintained with reproducibility, explainability, and domain alignment. These models are used where interpretability, small data, or tabular feature sets dominate.

---

## Principle Set

### 1. Use Cases Best Suited for Classical ML

- Use classical ML for:
  - SLA risk classification  
  - Time-series forecasting (e.g., ARIMA, XGBoost)  
  - Escalation triage scoring  
  - Outlier and fraud detection in structured signals  
- Avoid use for generative tasks or unstructured text unless in hybrid form

### 2. Feature Engineering is First-Class

- All models must include:
  - Feature definitions with semantic labels  
  - Feature types (numeric, categorical, ordinal)  
  - Transformations (e.g., log, binning, encoding)  
- Maintain feature lineage and source field mapping

### 3. Model Reproducibility

- Every experiment must be reproducible with:
  - Fixed random seeds  
  - Versioned training dataset snapshots  
  - Model code and parameters in Git  
  - Environment captured via requirements.txt or Conda

### 4. Evaluation Metrics Must Reflect Business Context

- For classification: precision, recall, AUC, F1  
- For forecasting: MAE, MAPE, RMSE, directionality accuracy  
- Include domain SLAs and cost of false positives/negatives in model selection

### 5. Explainability and Transparency

- Prefer interpretable models (e.g., logistic regression, tree ensembles) in regulated domains  
- For black-box models (e.g., SVM, XGBoost), apply SHAP, LIME, or permutation importance  
- Visualize top features and confidence thresholds in UI/reporting

### 6. Model Validation and Monitoring

- Use train/validation/test splits or time-aware cross-validation  
- Track concept drift and prediction stability over time  
- Monitor model inputs and outputs for anomalies, missing values, and range violations

### 7. Deployment Guidelines

- All deployed models must:
  - Be wrapped in versioned APIs  
  - Include schema contracts for input/output  
  - Fail gracefully with fallback strategies

### 8. Automation and CI/CD

- Automate training, testing, and packaging in CI pipelines  
- Validate schema changes to features before re-training  
- Auto-tag experiments, model artifacts, and metrics in MLFlow or SageMaker

### 9. Governance and Approval

- Maintain an inventory of deployed models with:
  - Owner, purpose, last training date  
  - Evaluation metrics  
  - Approval and sign-off status  
- Apply role-based controls for model promotion to production

### 10. Security, Data Sensitivity, and Compliance

- Mask sensitive features before persistence or export  
- Ensure reproducibility and auditability of all outputs  
- Align with data retention, encryption, and access control policies
