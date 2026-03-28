# EURUSD Microstructure Pattern Backtest

> A quantitative research pipeline for high-frequency Forex data — built with Python, pandas, and NumPy.

---

## Overview

This project demonstrates a **reproducible, end-to-end quantitative workflow** applied to a large-scale financial time-series dataset.

Using 1-minute EURUSD OHLC data (2015–2021), the pipeline:

- Ingests and cleans millions of rows of raw tick data
- Reconstructs higher-timeframe candles through time-series aggregation
- Detects specific candlestick microstructure patterns via vectorized logic
- Evaluates pattern outcomes through a custom NumPy-based backtesting engine
- Generates reproducible statistical reports for further research

The focus is on **data engineering quality, numerical performance, and research reproducibility** — skills directly applicable to financial data science and quantitative analysis roles.

---

## Stack

| Layer | Tool |
|---|---|
| Data processing | Python, pandas |
| Numerical computation | NumPy (vectorized) |
| Data source | Kaggle — EURUSD 1m (2015–2021) |
| Output | CSV datasets + statistical report |

---

## Workflow

### 1. Data Collection
- **Source:** [EURUSD 1-minute OHLC dataset (Kaggle)](https://www.kaggle.com/datasets/ankitjha420/forex-eurusd-1m-data-2015-to-2021)
- Over 3 million rows of raw tick data across multiple CSV files

### 2. Data Cleaning Pipeline
Raw files are merged and standardized through a multi-step cleaning script:

- Merge multiple CSVs into a single dataset
- Standardize column names and datetime parsing
- Remove duplicates and sort chronologically
- Export clean 1-minute and 2-minute aggregated datasets

**Outputs:** `EURUSD_1m_clean.csv`, `EURUSD_2m_clean.csv`

### 3. Pattern Detection
The algorithm scans every 2-minute candle for momentum structures with limited wick retracement:

**Bullish signal:**
```
close > open
upper_wick < body
close > previous_high
```

**Bearish signal:**
```
close < open
lower_wick < body
close < previous_low
```

When a signal is detected, the engine checks 1-minute data to simulate a limit order entry at the candle open price, evaluated at the close of the following 2-minute candle.

### 4. Backtesting Engine
A custom NumPy-based engine processes the full dataset with:

- Fully vectorized calculations — no Python loops over rows
- Memory-efficient array operations
- Automatic statistical report generation

**Output:** `backtest_results.txt`

---

## Results

| Metric | Value |
|---|---|
| Total trades | 81,938 |
| Win rate | 51.15% |
| Profit factor | 1.047 |
| Expectancy | +0.023 per trade |
| Std deviation | 0.9997 |

### Interpretation

The baseline pattern produces a **slight positive bias above random** (profit factor > 1.0, positive expectancy) — consistent with what raw microstructure signals typically yield before applying filters.

This result is the expected starting point for quantitative strategy research. The pipeline is designed to layer additional filters on top of this baseline, including:

- Session-based filtering (London / New York overlap)
- Volatility regime detection
- Spread and transaction cost modeling
- Multi-candle confirmation logic

The statistical framework ensures that each filter's marginal impact on edge can be measured and validated independently.

---

## Key Engineering Features

| Feature | Implementation |
|---|---|
| Large-scale data ingestion | Multi-file CSV merge with pandas |
| Time-series aggregation | 1m → 2m candle reconstruction |
| Vectorized pattern detection | NumPy boolean masking |
| Custom backtest engine | Array-based, loop-free computation |
| Reproducibility | Structured scripts, exported datasets, text reports |

---

## How to Run

**1. Download the dataset**

[EURUSD 1m data from Kaggle](https://www.kaggle.com/datasets/ankitjha420/forex-eurusd-1m-data-2015-to-2021) — place raw CSV files in the `data/` folder.

**2. Run the cleaning pipeline**
```bash
python clean_data.py
```
Outputs: `EURUSD_1m_clean.csv`, `EURUSD_2m_clean.csv`

**3. Run the backtest**
```bash
python backtest_numpy.py
```
Output: `backtest_results.txt`

---

## Project Structure

```
├── data/
│   ├── raw/                  # Raw CSV files from Kaggle
│   ├── EURUSD_1m_clean.csv   # Cleaned 1-minute data
│   └── EURUSD_2m_clean.csv   # Aggregated 2-minute data
├── clean_data.py             # Data cleaning pipeline
├── backtest_numpy.py         # NumPy backtesting engine
├── backtest_results.txt      # Statistical output
└── README.md
```

---

*Built as part of a quantitative data analysis portfolio. Dataset sourced from Kaggle under public license.*
