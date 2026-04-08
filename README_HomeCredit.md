# Credit Default Risk Classification – Home Credit

## Business Context

**Home Credit** is a non-banking financial institution founded in 1997 in the Czech Republic, operating in 14 countries including the US, Russia, Kazakhstan, China, and India. The company specializes in extending loans to people with **little or no credit history** — individuals who would otherwise be denied credit or exploited by predatory lenders.

**Scale:** 29M+ clients · €21B in total assets · 160M+ loans issued

Home Credit uses alternative data — telecom records, transaction history, demographic variables — to assess repayment capacity. The core risk problem: predicting whether a loan applicant will **default**, enabling smarter credit decisions while expanding financial inclusion.

**Objective:** Build a supervised classification model to predict loan default, supporting:
- Earlier identification of high-risk applicants
- Optimized credit authorization workflow
- Reduced non-performing loan (NPL) rate
- Deployment as a dashboard or REST API integrated with origination systems

---

## Dataset

**Source:** [Home Credit Default Risk – Kaggle](https://www.kaggle.com/c/home-credit-default-risk)

| File | Records | Features |
|---|---|---|
| `application_train.csv` | 307,511 | 122 + TARGET |
| `application_test.csv` | 48,744 | 122 |

Each record represents a loan application with demographic, employment, financial, and social variables. **Target variable:** `TARGET` (1 = default, 0 = no default).

**Class imbalance:** 92% no default / 8% default — critical design constraint for model selection and evaluation.

---

## Methodology (CRISP-DM)

### 1. Data Preparation
- Dropped `SK_ID_CURR` (non-predictive identifier)
- Separated numeric and categorical variables
- **Imputation:** `SimpleImputer(mean)` for numeric · `SimpleImputer(most_frequent)` for categorical
- **Label encoding** for all categorical variables
- **IQR outlier removal** on numeric features
- **Upsampling** of minority class to balance TARGET distribution
- 80/20 train/test split

### 2. Model Comparison

| Model | F1 Score | Precision | Recall |
|---|---|---|---|
| **XGBoost** (1,000 trees, max_depth=9) | **0.97** | **0.94** | **1.0** |
| Random Forest (100 trees, max_depth=20) | 0.92 | 0.92 | 0.92 |
| Decision Tree (Gini, max_depth=20) | 0.68 | 0.68 | 0.68 |
| Logistic Regression (L1) | 0.68 | 0.68 | 0.68 |

All models evaluated on: Accuracy · Precision · Recall · F1-Score · Confusion Matrix

---

## Key Findings

**1. XGBoost is the recommended model** — F1 of 0.97 and perfect recall (1.0), meaning zero missed defaulters on the test set. In a lending context, false negatives (missed defaults) are the most costly outcome.

**2. External scores dominate feature importance** — `EXT_SOURCE_3` and `EXT_SOURCE_2` (normalized scores from external bureaus/telecom providers) are the top predictors across Decision Tree, Random Forest, and XGBoost. These encode more default risk signal than any internal variable.

**3. Age and employment tenure are consistent top-5 predictors** — `DAYS_BIRTH` and `DAYS_EMPLOYED` appear in every model's feature importance chart. Older, longer-employed applicants default less.

**4. Missing documentation flags signal default risk** — XGBoost surfaces `FLAG_DOCUMENT_13`, `FLAG_DOCUMENT_16` as high-importance features, suggesting that incomplete documentation is itself a risk indicator.

**5. Despite extreme class imbalance (92/8)**, the combination of upsampling + XGBoost produces a highly reliable classifier, outperforming simpler models by a wide margin.

---

## Deployment Proposal

**Option A — Fast adoption:** Power BI or Tableau dashboard where analysts interact with model predictions visually. Lower cost, faster implementation.

**Option B — Full integration:** REST API that accepts applicant data and returns default probability, integrated directly into the credit origination system. Higher upfront investment, full automation potential.

---

## Future Work
- Apply SMOTE instead of simple upsampling for better synthetic minority generation
- Incorporate external credit history and transactional pattern data as additional features
- Hyperparameter tuning via GridSearchCV on XGBoost
- Experiment with model calibration to produce well-calibrated probability scores

---

## Stack

```
Python · Scikit-learn · XGBoost · Pandas · NumPy · Matplotlib · Seaborn
```
