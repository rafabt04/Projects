# Consumer Sentiment Analysis – Samsung, Apple & Huawei (Trustpilot)

## Business Context

This project analyzes consumer perception of three major smartphone brands — **Samsung, Apple, and Huawei** — using Trustpilot reviews. The goal was to quantify brand sentiment, compute NPS, identify the most common complaint and praise topics, and deliver a comparative dashboard for digital marketing strategy.

The analysis was conducted as part of a Digital Marketing course, focusing on **Online Brand Monitoring (OBM)** and the use of NLP to extract actionable insights from unstructured review data.

> **Important context:** Trustpilot is a complaint-driven platform — users are significantly more likely to leave reviews after negative experiences. The sentiment distribution reflects this bias and should not be interpreted as an overall measure of brand satisfaction.

---

## Scope

| Brand | Trustpilot Avg Rating | Positive | Neutral | Negative |
|---|---|---|---|---|
| Samsung | 1.83 | 374 | 38 | 574 |
| Apple | 1.97 | 450 | 47 | 473 |
| Huawei | — | — | — | — |

**Total reviews analyzed:** 1,478 across all three brands

---

## Methodology

### 1. Sentiment Classification – VADER
Each review's `Processed_Text` column is scored using **NLTK's VADER SentimentIntensityAnalyzer**, which produces a compound score from -1 (most negative) to +1 (most positive).

| Compound Score | Label |
|---|---|
| ≥ 0.05 | Positive |
| ≤ -0.05 | Negative |
| Between -0.05 and 0.05 | Neutral |

VADER was chosen for its effectiveness on short, informal consumer text — it handles punctuation, capitalization, and slang without requiring model training.

### 2. Net Promoter Score (NPS)
NPS is derived from star ratings:
- **Promoters** — Rating ≥ 4
- **Passives** — Rating = 3
- **Detractors** — Rating ≤ 2

```
NPS = ((Promoters - Detractors) / Total Respondents) × 100
```

### 3. Topic Classification
Reviews are tagged by topic using keyword matching across six categories:

| Topic | Example Keywords |
|---|---|
| Customer Service | service, support, warranty, refund, repair |
| Staff | staff, employee, unprofessional, technician |
| Price | price, cost, expensive, cheap, money |
| Ease of Use | easy, difficult, user-friendly, complex |
| Sales | sale, discount, offer |
| Product Quality | quality, broken, defective, performance |

A review can carry multiple topic tags. Frequency analysis surfaces the dominant pain points per brand.

### 4. Visualizations
- NPS bar chart (Promoters / Passives / Detractors)
- Sentiment distribution bar chart (Positive / Neutral / Negative)
- Topic frequency horizontal bar chart

### 5. Export
Processed data with sentiment labels and topic tags is exported to CSV for use in the comparative **Power BI dashboard**.

---

## Key Findings

- **Most complaints** across all three brands are related to **Customer Service and Staff**, not the products themselves — a consistent signal that after-sale support is the primary driver of negative reviews on Trustpilot
- Samsung had the highest proportion of negative reviews (574 vs. 374 positive on Trustpilot)
- Apple showed a slightly more balanced distribution despite a similar low average rating
- Topic frequency confirmed that product quality complaints were secondary to service-related issues across all brands

---

## Files

| File | Brand |
|---|---|
| `Samsung_sentiment_analysis_trustpilot.ipynb` | Samsung |
| `Apple_sentiment_analysis_trustpilot.ipynb` | Apple |
| `Huawei_sentiment_analysis_trustpilot.ipynb` | Huawei |

---

## Stack

```
Python · NLTK (VADER) · Pandas · Matplotlib · Seaborn · WordCloud
```
