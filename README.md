# Trader Performance vs Market Sentiment

> Analysis of how Bitcoin market sentiment (Fear & Greed Index) affects trader behavior and profitability on the Hyperliquid exchange.
>
> Completed as part of the **Primetrade.ai Data Science Internship Assignment**.

---

## Table of Contents

- [Project Objective](#project-objective)
- [Setup](#setup)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Data Sources](#data-sources)
- [Feature Engineering](#feature-engineering)
- [Methodology](#methodology)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Machine Learning Model](#machine-learning-model)
- [Key Insights](#key-insights)
- [Strategy Recommendations](#strategy-recommendations)
- [Author](#author)

---

## Project Objective

Analyze the relationship between Bitcoin market sentiment, trader behavior, and trading profitability using real Hyperliquid exchange data.

The project explores how traders change the following under different sentiment conditions:

| Metric | Description |
|---|---|
| Leverage | How much margin traders use |
| Trade Size | Size of individual positions |
| Long / Short Bias | Directional preference |
| Win Rate | Percentage of profitable trades |
| Profitability | Net PnL per sentiment regime |

---

## Setup

**Install dependencies:**

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

**Place datasets inside the `data/` folder:**

```
data/
├── fear_greed_index.csv
└── historical_data.csv
```

**Run the notebook:**

```bash
jupyter notebook trader-performance-vs-market-sentiment.ipynb
```

Then select `Kernel → Restart & Run All`.

---

## Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=flat)
![Seaborn](https://img.shields.io/badge/Seaborn-4C8CBF?style=flat)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-F7931E?style=flat&logo=scikitlearn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

---

## Project Structure

```
trader-performance-vs-market-sentiment/
│
├── trader-performance-vs-market-sentiment.ipynb   ← Main analysis notebook
├── README.md
│
├── charts/
│   ├── Chart 1 — Performance Metrics_Fear vs Greed.png
│   ├── Chart 2 — Trader Behavior_Fear vs Greed.png
│   ├── Chart 3 — Trader Segments (K-Means).png
│   ├── Chart 4 — Win Rate (%) by Sentiment.png
│   ├── Chart 5 — Directional Bias & Leverage.png
│   ├── chart6_pnl_fear_greed.png
│   └── prediction_results_random_forest.png
│
└── data/
    ├── historical_data.csv
    └── fear_greed_index.csv
```

---

## Data Sources

### Fear & Greed Index

Daily Bitcoin market sentiment values, grouped into three buckets:

| Bucket | Range |
|---|---|
| 🔴 Fear | Index < 40 |
| 🟡 Neutral | Index 40–60 |
| 🟢 Greed | Index > 60 |

Columns: `Date`, `Fear & Greed value`, `Sentiment classification`, `Timestamp`

### Trader Historical Data

Trading activity from the Hyperliquid exchange, aggregated daily.

Columns: `Account`, `Execution price`, `Trade size`, `Position direction`, `Closed PnL`, `Leverage`, `Timestamp`

---

## Feature Engineering

Daily metrics generated for analysis and modelling:

| Feature | Description |
|---|---|
| Daily PnL | Net profit or loss per day |
| Win Rate | % of profitable trades |
| Average Leverage | Mean leverage used |
| Number of Trades | Daily trade count |
| Average Trade Size | Mean position size |
| Long / Short Ratio | Directional bias |
| Fear & Greed Value | Raw index value |
| Market Sentiment | Fear / Neutral / Greed label |

---

## Methodology

1. Loaded Fear & Greed Index dataset
2. Loaded trader historical trading data
3. Aligned both datasets on date
4. Simplified sentiment into three categories: **Fear**, **Neutral**, **Greed**
5. Aggregated trading activity into daily performance metrics
6. Performed EDA across sentiment regimes
7. Built and evaluated a Random Forest classifier

---

## Exploratory Data Analysis

### Chart 1 — Performance by Sentiment
Compares average PnL, win rate, and profitability across sentiment regimes.
> **Finding:** Fear periods significantly reduce trader profitability.

### Chart 2 — Trader Behavior
Examines how traders adjust leverage, trade size, and trading frequency.
> **Finding:** Leverage increases significantly during greed markets.

### Chart 3 — Trader Segmentation (K-Means, k=3)
Traders clustered using win rate, leverage, trade frequency, position size, and long/short ratio.

| Cluster | Description |
|---|---|
| ✅ Consistent Winners | Controlled risk, lower leverage |
| ⚠️ Inconsistent Traders | Variable performance |
| ❌ Consistent Losers | High leverage, poor timing |

### Chart 4 — Win Rate by Sentiment
> **Finding:** Greed periods generally produce higher win rates.

### Chart 5 — Directional Bias & Leverage
> **Finding:** Greed markets increase long exposure and leverage.

### Chart 6 — PnL vs Fear & Greed Index
Visualizes how daily trading profitability changes with the raw sentiment index value.

---

## Machine Learning Model

A **Random Forest classifier** predicts next-day profitability.

```
Target:  1 → next day profit
         0 → next day loss
```

**Features used:** Sentiment category, Fear & Greed value, Average leverage, Win rate, Number of trades, Average trade size, Long/Short ratio

### Evaluation

| Metric | Result |
|---|---|
| Cross-validated accuracy | ~50–55% |
| Evaluation charts | Confusion matrix, ROC curve, Precision-Recall curve, Feature importance |

### Feature Importance (Ranked)

1. Win rate
2. Average trade size
3. Average leverage
4. Fear & Greed Index value

> Sentiment alone is weak but becomes more predictive when combined with trader behavior features.

---

## Key Insights

### 🔴 Fear Markets Reduce Profitability
- Lower win rates
- Negative average PnL
- Higher volatility

### 📈 Greed Drives High Leverage
Traders significantly increase leverage during greed periods, raising overall risk exposure.

### 🏆 Winning Traders Use Lower Leverage
Consistent winners typically use lower leverage, maintain controlled position sizing, and align trades with market sentiment.

---

## Strategy Recommendations

### Strategy 1 — Sentiment-Based Leverage Control

> When **Fear & Greed Index < 40**, cap maximum leverage at **5×**

Reduces the risk of over-leveraged trades during volatile market conditions.

### Strategy 2 — Sentiment-Aligned Direction

| Sentiment | Preferred Direction |
|---|---|
| 🔴 Fear | Short positions |
| 🟢 Greed | Long positions |

Improves win rate by reducing counter-trend trading.

---

## Author

**Saurabh Raj Varma**

[![Kaggle](https://img.shields.io/badge/Kaggle-20BEFF?style=flat&logo=kaggle&logoColor=white)](https://www.kaggle.com/saurabhrajvarma)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/SAURABHRAJVARMA)

---

*This project demonstrates how market psychology influences trading behavior and profitability.*
