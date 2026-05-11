# 🧠 Explainable AI for Financial Decision-Making

> **Self-Study Project - CSM961 | M.Tech Engineering Systems (CS) | DEI**  
> **Harsh Pathak (2501670)** · Supervised by Dr. Prem Sewak Sudhish

---

## 📌 Overview

This project explores the theory and practical implementation of **Explainable Artificial Intelligence (XAI)** in the context of financial decision-making - specifically **credit risk assessment**.

As black-box ML models (like XGBoost, DNNs) increasingly power high-stakes financial decisions, they raise critical concerns around **transparency, fairness, and regulatory compliance**. This study bridges that gap by applying state-of-the-art XAI techniques to make model predictions interpretable and auditable.

---

## 🎯 Motivation

- Modern deep learning models are opaque **"black boxes"** - hard to justify, audit, or trust
- Financial domains (loan approval, fraud detection, risk scoring) are highly regulated
- Regulations like **GDPR**, **EU AI Act**, and **Basel III** explicitly require model explainability
- XAI is one of the most critical barriers to responsible AI deployment today

---

## 📂 Repository Structure

```
.
├── self-study-german-credit.ipynb   # Main Jupyter Notebook (implementation)
├── README.md                        # Project overview (this file)
├── METHODOLOGY.md                   # Detailed methodology & XAI concepts
└── presentation/
    └── EndSem_Presentation.pptx     # End-semester presentation slides
```

---

## 🗃️ Dataset

**[Statlog (German Credit Data)](https://archive.ics.uci.edu/dataset/144/)** - UCI ML Repository

| Property | Details |
|----------|---------|
| Rows | 1,000 applicants |
| Features | 20 (mix of categorical & numerical) |
| Target | Credit risk: Good (1) / Bad (0) |
| Source | H. Hofmann, UCI ML Repository (1994) |

**Preprocessing steps performed:**
- Handling missing values
- Encoding categorical variables (one-hot / label encoding)
- Scaling numeric features (StandardScaler)
- Stratified train-test split

---

## ⚙️ Model

**XGBoost Classifier** was chosen for:
- High predictive performance (strong AUC & Recall)
- Ability to capture nonlinear feature relationships
- Widespread use in credit scoring research
- Inherent black-box nature → makes XAI application meaningful

---

## 🔍 XAI Techniques Applied

### SHAP (SHapley Additive exPlanations)
- Based on **Shapley values** from cooperative game theory
- Treats each feature as a "player" contributing to the prediction
- Provides both **global** (model-wide) and **local** (per-instance) explanations

| Plot | Purpose |
|------|---------|
| **Summary Plot** | Global feature importance ranked by mean \|SHAP\| value |
| **Force Plot** | Local explanation for a single applicant's prediction |
| **Waterfall Plot** | Step-by-step feature contributions for one prediction |

**Key findings:**
- `Credit Amount` is the most influential feature
- `Loan Duration` strongly increases default risk
- Low `Checking Account Balance` correlates with bad credit prediction

### PDP - Partial Dependence Plots
- Shows the **average effect** of a feature on the predicted outcome
- Marginalizes over all other features
- Used for **global sensitivity analysis**

**Key findings:**
- Longer loan duration → monotonically higher default probability
- Credit amount has a sharp risk inflection point beyond certain thresholds

---

## 📊 SHAP vs PDP - Complementary Approaches

| | SHAP | PDP |
|---|------|-----|
| Scope | Local + Global | Global |
| Question answered | *Why was this decision made?* | *How does this feature affect the model on average?* |
| Feature interactions | Captures them | Assumes independence |
| Best used for | Individual explanations, auditing | Sensitivity analysis, policy-making |

---

## 🚧 Limitations

- SHAP is **computationally expensive** on large datasets
- PDP assumes **feature independence** (can be misleading with correlated features)
- Feature interactions may not be fully captured
- Explanations still require **domain knowledge** to interpret correctly

---

## 🔭 Future Work

- Extend XAI to **LSTM / Transformer-based** financial models
- Quantitative comparison: **SHAP vs Counterfactual Explanations**
- Develop **domain-aware counterfactuals** respecting financial constraints
- Validate human interpretability through **user studies**
- Explore **fairness-aware and regulation-compliant** XAI frameworks

---

## 📚 References

1. Khan, F. S., et al. *"Model-Agnostic XAI Methods in Finance: A Systematic Review."* Artificial Intelligence Review, Springer, 2025.
2. Dwivedi, R., et al. *"Explainable AI (XAI): Core Ideas, Techniques, and Solutions."* ACM Computing Surveys, 2023.
3. Arrieta, A. B., et al. *"Explainable Artificial Intelligence (XAI): Concepts, taxonomies, opportunities and challenges."* Information Fusion, 2020.
4. Hofmann, H. *"Statlog (German Credit Data)."* UCI ML Repository, 1994. [DOI: 10.24432/C5NC77](https://archive.ics.uci.edu/dataset/144/)
5. Coursera. *"Machine Learning Specialization."* Stanford / DeepLearning.AI.
6. Piehl, H. *"XAI Concept Map."* Engines of Difference, 2024.

---

## 👤 Author

**Harsh Pathak**  
Roll No: 2501670  
M.Tech. Engineering Systems with Specialization in CS  
Dayalbagh Educational Institute (DEI)

**Supervisor:** Dr. Prem Sewak Sudhish  
Associate Professor, Dept. of Physics & Computer Science, Faculty of Science, DEI

---

*This project was completed as part of the Self-Study course CSM961.*
