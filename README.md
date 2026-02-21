# GPU Analysis

**Question:** How do crypto and financial markets correlate with GPU pricing and price-to-performance over time?

- **Data:** GPU specs/prices (from `gpu_price.json`), yearly Bitcoin, S&P 500 return, and **GPU supply chain** (TSMC fab, NVIDIA, AMD, Micron).
- **Flow:** EDA (data acquisition + cleaning) → correlation → regression → prediction → visualization.

## Structure

```
gpuAnalysis/
├── README.md
├── requirements.txt
├── data/
│   ├── README.md
│   ├── raw/          # gpu_data.csv, btc_yearly.csv, sp500_yearly.csv, supply_chain_yearly.csv
│   └── processed/    # gpu_clean.csv, gpu_yearly.csv, merged_yearly.csv
├── eda_cleaning.ipynb   # acquires all raw data + cleans GPU → gpu_clean, gpu_yearly
├── correlation.ipynb
├── regression.ipynb
├── prediction.ipynb
└── visualization.ipynb
```

## Workflow

1. **Run notebooks in order:** `eda_cleaning` (writes raw + processed) → `correlation` → `regression` → `prediction` → `visualization`. The EDA notebook exports GPU CSV and fetches BTC, S&P 500, and GPU supply chain (TSMC, NVIDIA, AMD, Micron) data via yfinance.

Run notebooks from project root; paths resolve to `data/raw` and `data/processed`.

## Data

- **raw:** Written by the EDA notebook: GPU CSV from `gpu_price.json`; BTC, S&P 500, and GPU supply chain (TSMC, NVIDIA, AMD, Micron) via yfinance. See `data/README.md`.
- **processed:** EDA notebook writes `gpu_clean.csv`, `gpu_yearly.csv`; correlation notebook writes `merged_yearly.csv` (includes supply chain returns when available).

**How you get `merged_yearly.csv`:** Run `eda_cleaning.ipynb` (creates raw files and `gpu_yearly.csv`), then run `correlation.ipynb`. Correlation merges GPU yearly with BTC, SP500, and supply chain data and saves `data/processed/merged_yearly.csv`.

## Requirements

```bash
pip install -r requirements.txt
```

Python 3.10+. Key deps: pandas, numpy, matplotlib, seaborn, yfinance, statsmodels, scikit-learn, jupyter.

## Research outline

- **EDA:** Acquire raw data (GPU export, BTC, SP500, GPU supply chain) then clean price/perf, perf_per_dollar, yearly medians.
- **Correlation:** Merge GPU yearly + BTC + SP500 + supply chain (TSMC, NVIDIA, AMD, Micron); Pearson heatmaps.
- **Regression:** OLS of median_gpu_price on BTC, SP500 return, and (when available) supply chain returns; log-scale and supply chain models.
- **Prediction:** 80/20 train/test by time; features include BTC, SP500, and supply chain returns; RMSE vs naive (previous year’s price).
- **Visualization:** Time series (BTC vs GPU price, vs perf-per-dollar); supply chain vs GPU price; scatter with regression line; findings.

**Avoid:** Many predictors on small N; claiming causation; ignoring lags. **Do:** Simple models; interpret coefficients; note volatility (e.g. 2017, 2021).
