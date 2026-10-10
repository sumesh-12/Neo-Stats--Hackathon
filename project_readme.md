# End-to-End Loan Refusal Prediction & Intelligence Suite

An enterprise-grade machine learning pipeline and interactive executive dashboard suite built for automated credit risk scoring and refusal prediction. This project leverages an optimized **XGBoost Classifier** paired with an interactive 3-page Power BI executive suite to assist credit officers in default prevention.

---

## 🚀 Key Project Highlights

* **High Predictive Performance**: Achieved a **Validation AUC of 0.9142** and an exceptional **Out-of-Time (OOT) Test AUC of 0.9375**, proving robust temporal stability and generalization.
* **Rigorous Data Hygiene**: Systematic elimination of post-decision data leakage (such as `CODE_REJECT_REASON`) to ensure true predictive validity.
* **Conservative Threshold Tuning**: Operationalized at a **0.75 decision threshold** to prioritize high precision and minimize costly false credit approvals.
* **Executive Decision Support**: A 3-page interactive Power BI dashboard featuring dynamic risk banding (`High`, `Medium`, `Low`) and global cross-filtering.

---

## 📊 Evaluation Metrics Summary

| Dataset Split | ROC-AUC | Threshold | Accuracy | Precision | Recall | F1-Score |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Validation** | `0.9142` | `0.75` | `0.8920` | `0.8250` | `0.7830` | `0.8034` |
| **Out-of-Time (OOT)** | `0.9375` | `0.75` | `0.9015` | `0.8410` | `0.7950` | `0.8174` |

---

## 🛠️ Technical Pipeline & Methodology

### 1. Data Understanding, Integration & Leakage Handling
* **Relational Merging**: Consolidated multi-source transactional and demographic tables (`fact_applications`, `dim_client`) using unique primary keys (`SK_ID_PREV`, `SK_ID_CURR`).
* **Post-Decision Leakage Removal**: Identified and purged post-decision variables (e.g., `CODE_REJECT_REASON`) that only manifest *after* a loan evaluation occurs, preventing target leakage and ensuring the model predicts future risk rather than past manual decisions.

### 2. Feature Engineering & Domain Proxies
* **`RATIO_APP_TO_INCOME`**: Engineered financial stress ratio capturing debt burden relative to applicant earnings.
* **Behavioral Indicators**: Extracted customer loyalty flags (`NAME_CLIENT_TYPE_Repeater`) and credit insurance preferences (`NFLAG_INSURED_ON_APPROVAL`).
* **Categorical Banding**: Grouped continuous distributions into non-linear demographic and income segments (`AGE_BAND`, `INCOME_BAND`).

### 3. Model Selection: Why XGBoost?
* **Superior Tabular Performance**: Outperformed baseline models (Logistic Regression) by natively handling complex non-linear feature interactions.
* **Missing Value & Imbalance Management**: Handled class imbalances and sparse application fields natively during tree construction.
* **Interpretability**: Provided actionable global feature importance scores to satisfy regulatory transparency standards.

### 4. Top Predictive Risk Drivers
1. **`NAME_PORTFOLIO_POS`**: Point-of-Sale division indicator (#1 risk driver).
2. **`NFLAG_INSURED_ON_APPROVAL`**: Insurance requirement flag upon approval.
3. **`NAME_CLIENT_TYPE_Repeater`**: Historical customer engagement status.
4. **`RATIO_APP_TO_INCOME`**: Debt-to-income financial strain proxy.

---

## 📈 Power BI Interactive Dashboard Structure

* **Page 1 (EDA & Business Overview)**: Monitors baseline refusal rates (~21.7%) and application volumes across product portfolios (`NAME_PORTFOLIO_POS`) and demographic bands using cross-filtering slicers.
* **Page 2 (Model Performance)**: Visualizes validation metrics, the ROC curve, and decision threshold configurations to establish technical trust.
* **Page 3 (Predictions & Risk Scoring)**: Actionable frontline risk-scoring table featuring dynamic `RISK_BAND` conditional color formatting (`High`, `Medium`, `Low`) for credit officers.

---

## 📁 Repository Structure

```text
├── data/                    # Cleaned datasets and feature CSVs
├── notebooks/               # Jupyter notebooks for data engineering & training
├── models/                  # Serialized XGBoost model (`xgboost_loan_model.pkl`)
├── dashboard/               # 3-page interactive Power BI executive suite (.pbix)
├── outputs/                 # Evaluation metrics CSV (`model_evaluation_metrics.csv`)
└── README.md                # Project documentation
```

---
*Built for the NeoStats Hackathon Submission.*