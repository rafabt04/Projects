# Data Analytics & Machine Learning Projects

Business analytics and machine learning projects developed during my Master's in Business Analytics at EGADE Business School (ITESM). Each project applies a different analytical methodology to a real or industry-standard dataset, with a focus on extracting actionable business insights.

---

## Projects

### 1. 🏦 Credit Risk Classification – LendingClub *(HBS Case Series)*
**`LendingClub/Lending Club.ipynb`**

Implementation of the full Harvard Business School LendingClub case series (A/B/C) by Srikant M. Datar & Caitlin N. Bowler (HBS Case 119-020, 2018). Predicting loan default on 1.4M+ records using four classification models. Random Forest achieved 84% accuracy, validated via 5-fold cross-validation. Interest rate was identified as the strongest default predictor.

**Stack:** Python · Scikit-learn · XGBoost · Pandas · Matplotlib · Seaborn

---

### 2. 🛒 Market Basket Analysis – Pollolandia / Grupo Gamer
**`GrupoGamer/Market Basket Analysis.ipynb`**

Association rule mining on 33,833 **real** point-of-sale transactions from a Pollolandia franchise restaurant in Honduras (Q4). Applied the Apriori algorithm to identify product affinity pairs and a Chi-square test to determine complementarity vs. substitutability of key menu items.

**Stack:** Python · mlxtend · SciPy · Pandas · Matplotlib

---

### 3. 🤖 Food Recommender System Prototype – Balanced bAIt
**`BalancedBAIt/Recommender_system_UC_Irvine.ipynb`**

SVD-based food recommendation prototype for users with insulin resistance. Built on the UC Irvine Recipe Reviews dataset, the system maps user preference patterns to recipe recommendations using Pearson correlation in a 5-dimensional latent space. Developed as part of a startup concept targeting Mexico's ~61% insulin resistance prevalence.

**Stack:** Python · Scikit-learn (TruncatedSVD) · Pandas · NumPy · ucimlrepo

---

### 4. 📊 Consumer Sentiment Analysis – Samsung, Apple & Huawei (Trustpilot)
**`SentimentAnalysis/`**

NLP sentiment analysis on 1,478 Trustpilot reviews across three major smartphone brands. Applies VADER sentiment classification, NPS calculation from star ratings, and keyword-based topic tagging to identify the primary drivers of positive and negative brand perception. Results fed into a comparative Power BI dashboard for digital marketing strategy.

**Stack:** Python · NLTK (VADER) · Pandas · Matplotlib · Seaborn · Power BI

---

### 5. 🏢 Direct Debit Propensity Prediction – Allianz Insurance
**`Allianz/Allianz_direct_debit.ipynb`**

Binary classification model to predict which Allianz policyholders are likely to adopt direct debit payment — addressing the company's 21% adoption rate vs. a 65% industry average. Trained on 452,220 insurance contracts across 14 variables. Random Forest achieved F1 of 0.826. A second experiment removes product variables to test propensity prediction from customer demographics alone.

**Stack:** Python · Scikit-learn · Pandas · NumPy · Matplotlib · Seaborn

---

## About

**Rafael Bravo** · Business Analytics & Operations Manager

[![LinkedIn](https://img.shields.io/badge/LinkedIn-rafabt04-blue?logo=linkedin)](https://www.linkedin.com/in/rafabt04)

> All projects were developed with a strong emphasis on honest methodology, reproducibility, and business applicability.
