# Food Recommender System – Balanced bAIt

## Business Context

In Mexico, approximately **61% of the general population** shows some level of insulin resistance (PubMed Central) — a figure higher than most other countries and a key driver of Type 2 diabetes progression. Managing this condition requires consistent dietary discipline, personalized to each individual's metabolic profile, activity level, and food preferences.

**Balanced bAIt** is a startup concept built to address this gap through three integrated services:

1. **Specialist connection** — match users with bariatric doctors and nutritionists in their area for medical evaluation
2. **Personalized meal planning** — generate weekly menus using AI, tailored to the user's health profile and preferences
3. **Meal delivery** — deliver recommended meals to the user's home

This notebook is the **core AI component** of service #2 — the recommendation engine.

---

## How the Recommender Works

The engine uses **Truncated SVD (Singular Value Decomposition)**, a collaborative filtering technique that:

- Decomposes the user-recipe interaction matrix into a compact latent space
- Identifies hidden preference patterns shared across users
- Uses Pearson correlation in that latent space to find recipes with similar appeal profiles
- Scales efficiently to large datasets and supports real-time updates with new user data

---

## Data Architecture (Production)

In the full product, the model would be trained on four input layers:

| Layer | Variables |
|---|---|
| **User health profile** | Age, sex, BMI, body fat %, HbA1c, fasting glucose, lipid profile, physical activity level, medical history |
| **Nutritional data** | Glycemic Index (GI), Glycemic Load (GL), macronutrients, fiber, micronutrients (Mg, Cr), allergens, portion size, dietary tags (keto, Mediterranean, vegan) |
| **Behavioral data** | Food preferences, meal timing patterns, consumption history, satiety and post-meal glucose response |
| **Clinical guidelines** | ADA and WHO dietary recommendations for diabetes and prediabetes management |

**Production data sources:** USDA FoodData Central · Open Food Facts · National dietary databases

---

## This Prototype

Built on the **UC Irvine Recipe Reviews and User Feedback dataset** (Ali et al., 2023) as a proxy for the proprietary food database.

> The methodology is production-ready. The dataset would be replaced with curated low-glycemic recipes and real user health data in deployment.

### Pipeline

| Step | Description |
|---|---|
| Data fetch | Load dataset via `ucimlrepo` (id=911) |
| Feature selection | `recipe_code`, `user_id`, `recipe_name`, `thumbs_up`, `thumbs_down` |
| Score engineering | `score = thumbs_up - thumbs_down` |
| Normalization | MinMaxScaler → 1–5 rating scale |
| Utility matrix | Pivot: users × recipes, filled with normalized scores |
| Transposition | Recipe × user matrix for item-based filtering |
| SVD | TruncatedSVD with `n_components=5` latent dimensions |
| Similarity | Pearson correlation matrix across recipes in latent space |
| Output | Top recommendations for a seed recipe (correlation 0.97–0.99) |

---

## Stack

```
Python · Scikit-learn (TruncatedSVD) · Pandas · NumPy · ucimlrepo
```

---

## Reference

Ali, A., Matuszewski, S., & Czupyt, J. (2023). *Recipe Reviews and User Feedback* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5FG95
