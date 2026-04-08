# Credit Risk Classification – LendingClub (2007–2018)
### Harvard Business School Case Series · Datar & Bowler (2018)

> *Based on the Harvard Business School case series "LendingClub (A/B/C): Data Analytic Thinking, Decision Trees & Random Forests, and Gradient Boosting & Payoff Matrix" by Srikant M. Datar and Caitlin N. Bowler. HBS Case 119-020, August 2018.*
>
> Cases available at: https://www.hbs.edu/faculty/Pages/item.aspx?num=54844

---

## Business Context

LendingClub operates a peer-to-peer (P2P) lending platform connecting borrowers and lenders, eliminating traditional bank intermediaries. While attractive for investors seeking interest income and borrowers seeking quick liquidity, the model carries significant credit risk.

This notebook follows the full three-part HBS case series:
- **Case A** — Data Analytic Thinking: understanding the data, preparation, cross-validation
- **Case B** — Decision Trees & Random Forests
- **Case C** — Gradient Boosting (XGBoost) & Payoff Matrix

This project evaluates whether machine learning models can reliably predict loan default probability, supporting smarter investment and risk management decisions on the platform.

---

## Dataset

- **Source:** LendingClub public loan data (2007–2018 Q4)
- **Size:** 1.4M+ records after cleaning
- **Features selected:** 20 variables including `loan_amnt`, `int_rate`, `annual_inc`, `fico_range_high`, `dti`, `revol_util`, `term`, `home_ownership`, among others
- **Target variable:** `loan_status` → binary (0 = performing, 1 = default/charged off)

---

## Methodology

### 1. Data Cleaning
- Removed columns with more than 750,000 null values
- Dropped rows with remaining nulls after column filtering
- Removed `member_id` as a non-predictive identifier

### 2. Feature Engineering & Encoding
- Mapped `loan_status` to binary: `Fully Paid / Current / In Grace Period → 0`, `Late / Default / Charged Off → 1`
- Label encoded categorical variables: `application_type`, `home_ownership`, `term`, `verification_status`

### 3. Class Imbalance Handling
- Dataset was heavily imbalanced (~88% performing, ~12% default)
- Applied **upsampling** (sklearn `resample`) to balance classes to 1,438,202 records each

### 4. Outlier Removal
- Applied **IQR method** across numerical features to remove extreme values

### 5. Correlation Analysis
- Generated heatmap to validate feature independence before modeling

### 6. Models Compared

| Model | Train Accuracy | Test Accuracy |
|---|---|---|
| Logistic Regression (L1) | 64% | 64% |
| Decision Tree (Gini, max_depth=20) | ~90% | 71% |
| **Random Forest (100 trees, max_depth=20)** | **~90%** | **84%** |
| XGBoost (1000 estimators, max_depth=9) | ~99% | 81% |

### 7. Validation
- Random Forest validated via **5-fold cross-validation** on training set
- Evaluated with: Accuracy, F1-Score, Precision, Recall, Confusion Matrix, Log Loss

---

## Key Findings

- **Random Forest** was selected as the recommended model with **84% test accuracy** and the best log loss among all candidates
- **Interest rate (`int_rate`)** was the single most important feature across Random Forest, XGBoost, and Decision Tree models — a borrower's assigned rate already encodes much of their default risk
- Decision Tree showed signs of overfitting (high train, lower test accuracy)
- XGBoost performed well (81%) but didn't justify its added complexity over Random Forest

---

## Stack

```
Python · Pandas · NumPy · Scikit-learn · XGBoost · Matplotlib · Seaborn
```
