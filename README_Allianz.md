# Direct Debit Propensity Prediction – Allianz Insurance

## Business Context

**Allianz** is a multinational financial services group offering life, health, and non-life insurance products and asset management to 88M+ clients across 70+ countries.

**The problem:** Only **21% of Allianz insurance contracts** use direct debit as a payment method, compared to an industry average of **65%**. This gap has direct consequences:

- Reduced working capital efficiency
- Less predictable cash flow
- Higher administrative overhead from manual payment follow-up and collections

**The objective:** Build a machine learning classification model to predict which policyholders are likely to adopt direct debit, enabling targeted outreach and closing the adoption gap.

---

## Dataset

- **Size:** 452,220 insurance contract records
- **Variables:** 14 features (from 18 original, after dropping broker identifiers and non-predictive IDs)

| Variable | Description |
|---|---|
| `cs` | Customer Segment (Midcorp, Retail, SME) |
| `lob` | Line of Business (Property, Motor, Liability, Engineering, Accident) |
| `ptype` | Product Type |
| `premium` | Annual Premium |
| `age` | Customer Age group |
| `type` | Customer Type (Physical person, Enterprise) |
| `region` | Customer Region (BRU, WAL, FLA) |
| `prov` | Customer Province |
| `urban` | Customer Urbanization (Urban, Rural) |
| `y` | **Target** — Is Direct Debit (1 = yes, 0 = no) |

---

## Methodology (CRISP-DM)

### 1. Data Preparation
- Removed 0 duplicate contracts (verified)
- Dropped broker variables and payment frequency
- Label encoded all categorical variables
- **Upsampled** minority class (`y=1`) to 357,121 records to balance classes
- **IQR outlier removal** on `premium`
- 70/30 train/test split

### 2. Models — Part 1 (With Product Variables)

| Model | F1 Score | Log Loss |
|---|---|---|
| **Random Forest (10 trees)** | **0.826** | **6.27** |
| Decision Tree (Gini, max_depth=3) | 0.648 | 12.67 |
| Logistic Regression | 0.626 | 13.47 |

### 3. Models — Part 2 (Without Product Variables)
A second experiment removes `premium` and `ptype` to test whether customer demographics alone carry predictive power — answering whether the model can score propensity from CRM data without product details.

### 4. Confusion Matrix Comparison (Best Models)

|  | Random Forest | Decision Tree |
|---|---|---|
| True Positives | 80,700 | 73,200 |
| True Negatives | 98,200 | 65,900 |

---

## Key Findings

**1. Annual premium is the strongest predictor** across all models — policyholders with higher premiums are significantly more likely to use direct debit, likely because the financial benefit of automation is more tangible at higher payment amounts.

**2. Customer demographics carry independent signal** — without product variables, `age`, `region`, `province`, and `customer_type` still drive predictions, enabling propensity scoring from CRM data alone.

**3. High-propensity customer profile (from EDA):**
- Segment: Retail
- Type: Physical person
- Line of Business: Motor insurance
- Age: 40–69
- Product: Premium tier
- Broker Region: FLA
- Customer Location: Rural

**4. Recommended model: Random Forest** — best F1 (0.826), lowest log loss (6.27), no overfitting.

---

## Business Impact

| Benefit | Internal | External |
|---|---|---|
| Better working capital management | ✓ | |
| More predictable cash flow | ✓ | |
| Reduced administrative costs (collections) | ✓ | |
| No late payment charges | | ✓ |
| Simpler payment experience | | ✓ |

Scoring the existing portfolio with this model allows Allianz to prioritize direct debit outreach to the highest-propensity policyholders — directly addressing the 21% → 65% adoption gap.

---

## Stack

```
Python · Scikit-learn · Pandas · NumPy · Matplotlib · Seaborn
```
