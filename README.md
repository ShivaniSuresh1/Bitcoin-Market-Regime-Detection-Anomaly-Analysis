# Bitcoin Market Regime Detection & Anomaly Analysis

> Uncovering hidden structure in Bitcoin price dynamics using unsupervised learning, anomaly detection, and change-point analysis.

**Start here: [`main_notebook.ipynb`](main_notebook.ipynb)**

**Video: [Project Video](https://youtu.be/v8PicaSg21M?si=fjuLhISDmrwmNr4L)** 

---

## Overview

Rather than trying to predict Bitcoin prices, this project asks a more fundamental question: *what kinds of market behavior actually exist in Bitcoin data, and when do they occur?* Using 200,000+ minutes of high-frequency Bitcoin (BTC/USD) price data, I applied a multi-method data mining pipeline - feature engineering, K-Means clustering, Isolation Forest anomaly detection, SHAP explainability, and binary segmentation change-point detection - to identify distinct market regimes and understand how the market transitions between them.

The core finding: Bitcoin market behavior is **hierarchically structured**. Broad regimes capture distinct volatility and return profiles; transition probabilities reveal which regimes are stable vs. transient; and change points expose fine-grained structural shifts *within* regimes. Anomalies aren't random - they cluster in time around regime transitions, suggesting that market instability is episodic rather than continuous.

---

## Research Questions

1. Can unsupervised clustering identify meaningful, distinct market regimes in Bitcoin's high-frequency price data?
2. What features drive anomaly detection - is it extreme volatility, or something else?
3. How do market regimes transition over time, and which regimes are most stable?
4. Do structural change points in the volatility signal align with visually observable market events?

---

## Repository Structure

```
├── main_notebook.ipynb          # 📍 Main deliverable - start here
├── requirements.txt             # Full dependency list (exported from Colab)
├── README.md                    # You are here
├── checkpoints/
│   ├── checkpoint_1.ipynb       # Project Checkpoint 1
│   └── checkpoint_2.ipynb       # Project Checkpoint 2
└── data/
    └── README_data.md           # Instructions for downloading the dataset
```

---

## Dataset

**Source:** [Bitcoin Historical Data (1-min resolution)](https://www.kaggle.com/datasets/mczielinski/bitcoin-historical-data) - Kaggle

**Contents:** Open, High, Low, Close prices + trading volume, timestamped at 1-minute intervals over multiple years.

**Why this dataset?**
- Large-scale, spanning multiple market cycles
- Fine-grained (minute-level) - ideal for trend and anomaly detection
- Real-world, non-synthetic financial data
- Highly volatile and non-linear - well-suited for ML and data mining

**Preprocessing:**
- Removed missing values and zero-volume entries (non-trading periods)
- Trimmed to the last 200,000 rows for computational feasibility in Colab
- Engineered 6 features: log returns, short- and medium-term rolling volatility, volume change rate, price range, and Bollinger Band width
- Applied `StandardScaler` before modeling

The dataset is accessed via `kagglehub` directly in the notebook - no manual download needed.

---

## Methods

| Step | Method | Purpose |
|------|--------|---------|
| Feature Engineering | Rolling stats, log returns, Bollinger Bands | Capture momentum, risk, and liquidity |
| Anomaly Detection | Isolation Forest + Z-score ensemble | Flag statistically unusual market behavior |
| Explainability | SHAP (TreeExplainer) | Identify which features drive anomaly predictions |
| Regime Detection | K-Means (k=4) + PCA visualization | Cluster market behavior into interpretable states |
| Transition Analysis | Markov-style transition probability matrix | Quantify regime stability and switching dynamics |
| Change-Point Detection | Binary Segmentation (`ruptures`, L2 model) | Locate structural breaks in the volatility signal |
| Anomaly Density | Rolling window (n=500) over time | Show temporal clustering of anomalous periods |

---

## Key Results

- **4 market regimes** identified: two stable low-volatility states (Regimes 0 & 2), one high-volatility bullish phase (Regime 1), and one transitional/bearish state (Regime 3).
- **~1.37% of observations** flagged as anomalous; these cluster overwhelmingly in Regime 1 (~24.9% anomaly rate) vs. 0% in Regimes 0 and 2.
- **SHAP analysis** reveals a counterintuitive finding: in Bitcoin markets, *unusually low* volatility and subdued activity are the strongest anomaly signals - the model detects "unusual calmness," not extreme spikes.
- **Regime 2 is the most stable** (93% self-transition probability); Regime 3 is the most transient (30%), frequently flowing back into Regime 0.
- **Change points cluster** around the sharp mid-series price drop, confirming the model detects genuine structural shifts rather than noise.
- **Anomaly bursts** temporally align with regime transitions and structural breaks, confirming that market instability is episodic.

---

## How to Reproduce

This project was built entirely in **Google Colab**.

1. Open `main_notebook.ipynb` in [Google Colab](https://colab.research.google.com/)
2. Run all cells top-to-bottom - the notebook installs its own dependencies via `!pip install` in the first cell
3. Data is loaded automatically via `kagglehub` (you'll need a Kaggle account and API token configured)
4. All outputs (plots, tables, metrics) are generated inline

To install dependencies locally instead:
```bash
pip install -r requirements.txt
```

---

## Key Dependencies

| Package | Version |
|---------|---------|
| Python | 3.11 |
| pandas | 2.2.2 |
| numpy | 2.0.2 |
| scikit-learn | 1.6.1 |
| shap | 0.51.0 |
| ruptures | 1.1.10 |
| matplotlib | 3.10.0 |
| seaborn | 0.13.2 |
| kagglehub | 1.0.0 |

See `requirements.txt` for the complete pinned dependency list.

---

## Checkpoints

The `checkpoints/` folder contains earlier versions of this work showing the progression across the semester:
- `checkpoint_1.ipynb` - Initial data exploration and feature engineering
- `checkpoint_2.ipynb` - Early modeling results and intermediate findings
