# 📖 Methodology — XAI for Financial Decision-Making

This document provides an in-depth look at the concepts, pipeline, and XAI taxonomy used in this project.

---

## 1. What is Explainable AI (XAI)?

> *"Given a certain audience, explainability refers to the details and reasons a model gives to make its functioning clear or easy to understand."*

XAI acts as a **bridge between human understanding and machine decision-making**. The level and type of explanation must be adapted to the audience:

| Audience | Key Needs |
|----------|-----------|
| Domain Experts | Causality, robustness, feature validity |
| Regulators | Fairness, compliance, auditability |
| End Users | Trust, accessibility, actionable feedback |

---

## 2. ML Pipeline — Traditional vs. XAI-Enhanced

### Traditional Pipeline
```
Data → Preprocessing → Model Training → Prediction
```

### XAI-Enhanced Pipeline
```
Data → Preprocessing → Model Training → Prediction
                                              ↓
                               [ Understanding Phase ]
                               - Feature importance
                               - Bias detection
                               - Robustness analysis
                                              ↓
                               [ Explaining Phase ]
                               - Communicate how/why
                                 decisions are made
```

---

## 3. Taxonomy of XAI Techniques

### 3.1 By Transparency
| Type | Description | Examples |
|------|-------------|---------|
| **White-box** | Transparent logic, easy to inspect | Linear models, Decision Trees |
| **Black-box** | Opaque internals, high complexity | Neural Networks, Ensemble models |

### 3.2 By Model Dependency
| Type | Description | Examples |
|------|-------------|---------|
| **Model-specific** | Designed for one algorithm | Tree feature importance, Linear coefficients |
| **Model-agnostic** | Works with any model | LIME, SHAP |

### 3.3 By Scope
| Type | Question answered | Examples |
|------|------------------|---------|
| **Global** | "How does the model work in general?" | SHAP Summary, PDP, Surrogate Models |
| **Local** | "Why was *this* prediction made?" | LIME, SHAP Force Plot, Counterfactuals |

---

## 4. Feature-Based XAI Techniques Summary

| Technique | Description | Scope |
|-----------|-------------|-------|
| Feature Importance | Assigns weights via permutation methods | Global |
| PDP & ICE Plots | Average or instance-level effect of features | Global & Local |
| ALE Plots | Handles correlated features better than PDP | Global |
| Global Surrogate | Interpretable model approximating black-box | Global |
| LIME | Local linear approximation around data point | Local |
| SHAP Values | Game-theoretic feature contributions | Local + Global |

---

## 5. Why Finance Needs XAI

Financial decisions are among the **highest-stakes** applications of ML:

- **Loan approvals** affect people's livelihoods
- **Fraud detection** must minimize both false positives and false negatives
- **Credit scoring** can impact access to housing, education, and business

### Regulatory Requirements
| Regulation | Requirement |
|-----------|-------------|
| **GDPR (EU)** | Right to explanation for automated decisions |
| **EU AI Act** | High-risk AI systems must be transparent and auditable |
| **Basel III/IV** | Model risk management, stress testing, explainability |

---

## 6. SHAP — Deep Dive

### Theoretical Foundation
SHAP is grounded in **cooperative game theory**. The Shapley value for feature $i$ is:

$$\phi_i = \sum_{S \subseteq F \setminus \{i\}} \frac{|S|!(|F|-|S|-1)!}{|F|!} \left[ f_{S \cup \{i\}}(x_{S \cup \{i\}}) - f_S(x_S) \right]$$

Where:
- $F$ = set of all features
- $S$ = subset of features excluding $i$
- $f_S$ = model trained on subset $S$

### Properties
- **Efficiency**: Sum of SHAP values equals the difference between prediction and baseline
- **Symmetry**: Equal features get equal contributions
- **Dummy**: Irrelevant features get zero contribution
- **Additivity**: Ensemble SHAP values are additive

### Reading the Summary Plot
```
Y-axis  → Features ranked by mean |SHAP value|
X-axis  → SHAP value (negative = pushes toward "Bad", positive = "Good")
Color   → Feature value (Red = High, Blue = Low)
Width   → Distribution of SHAP values across all samples
```

---

## 7. PDP — Deep Dive

### How PDPs Work
For feature $x_s$, the partial dependence is:

$$\hat{f}_{x_s}(x_s) = \mathbb{E}_{x_c} \left[ \hat{f}(x_s, x_c) \right] = \int \hat{f}(x_s, x_c) \, d\mathbb{P}(x_c)$$

In practice, this is estimated by averaging predictions over all data points while varying only $x_s$.

### Limitations of PDP
- Assumes **feature independence** — can be misleading with correlated features
- ALE (Accumulated Local Effects) plots are a more robust alternative when features are correlated

---

## 8. Implementation Notes

### Environment
- Python 3.x
- Key libraries: `xgboost`, `shap`, `scikit-learn`, `matplotlib`, `pandas`, `numpy`

### Running the Notebook
```bash
# Install dependencies
pip install xgboost shap scikit-learn matplotlib pandas numpy jupyter

# Launch notebook
jupyter notebook self-study-german-credit.ipynb
```

### Key Hyperparameters (XGBoost)
The model was tuned with emphasis on:
- **AUC-ROC** as the primary metric (handles class imbalance well)
- **Recall** for the minority class (bad credit) — important in risk assessment

---

*For the full visual analysis, see the [presentation slides](presentation/EndSem_Presentation.pptx).*
