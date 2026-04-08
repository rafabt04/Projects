# Market Basket Analysis – Pollolandia / Grupo Gamer

## Business Context

**Grupo Gamer** is a Honduran company specializing in food franchise acquisition, currently operating 15+ restaurants and 150+ employees across Honduras. One of its franchises is **Pollolandia**, a fast food chain founded in 2002 in Guatemala City with 1,000+ locations across Guatemala, Honduras, Costa Rica, and El Salvador.

The goal of this project was to apply market basket analysis to Pollolandia's real transaction data to identify product affinity patterns and support cross-sell strategy decisions — specifically to evaluate whether key menu items were complementary or substitute goods.

---

## Dataset

- **Source:** Proprietary POS data from a Pollolandia restaurant (southern Honduras)
- **Period:** Q4 (last 3 months of the year)
- **Size:** 33,833 transactions · 19 original columns · 0 null values
- **Key fields used:** `Número de Factura`, `Nombre Producto`, `Cantidad`

---

## Methodology

### 1. Data Transformation
- Removed non-analytical columns: `Tipo de Orden`, `Id Empleado`, `Id de Producto`, `Costo`, `Precio`, `ISV`, `Id Cierre`, `Efectivo`, `Total Tarjeta`
- Formatted `Fecha` as datetime and extracted `dia_semana` (day of week)
- Grouped by `Número de Factura` × `Nombre Producto`, summed `Cantidad`
- Pivoted to invoice-product matrix and filled nulls with 0
- Binarized matrix: any quantity ≥ 1 → 1, else 0

### 2. Apriori Algorithm
- Applied Apriori with `min_support = 0.05`
- Generated association rules filtered by `lift > 1`
- Ranked rules by lift to identify non-random product associations

### 3. Chi-Square Hypothesis Test
- **H₀:** Tortillas, salad, and fries are substitute goods (no dependence)
- **H₁:** They are complementary goods (dependence exists)
- Built crosstab of the three products across all invoices
- Ran `chi2_contingency` from SciPy

---

## Results

### Top 10 Products by Support

| Product | Support |
|---|---|
| Frito Medio Pollo | 0.255 |
| Tortillas Paquete (5) | 0.241 |
| Papas en Bolsita | 0.230 |
| Frito Pechuga | 0.173 |
| Frito 8 Piezas | 0.153 |
| Frito Ala | 0.143 |
| Frito Muslo | 0.139 |
| Frito Pierna | 0.135 |
| Pepsi 600 ml | 0.100 |
| Ensalada | 0.082 |

### Key Association Rules (by Lift)

| Antecedent | Consequent | Lift |
|---|---|---|
| Frito Muslo | Frito Pierna | 3.26 |
| Frito Pierna | Frito Muslo | 3.26 |
| Frito Pechuga | Frito Ala | 2.65 |
| Frito Ala | Frito Pechuga | 2.65 |
| Tortillas Paquete (5) | Papas en Bolsita | 1.36 |

### Hypothesis Test Result
- **χ² = 11,786.06 · p = 1.0 · df = 14,082**
- **Conclusion: H₀ not rejected** — Tortillas, salad, and fries are **substitute goods**; customers tend to choose one, not combine them
- Business implication: bundling these three items in a combo is unlikely to drive incremental sales — bundling should focus on confirmed complementary pairs (e.g., chicken cuts + tortillas)

---

## Stack

```
Python · Pandas · mlxtend · SciPy · Matplotlib · Seaborn
```
