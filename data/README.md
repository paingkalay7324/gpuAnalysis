# Data

- **raw/** — Written by `eda_cleaning.ipynb` (data acquisition section). Do not edit after acquisition.
  - `gpu_data.csv` — GPU specs/prices (exported from `gpu_price.json`).
  - `gpu_price.json` — Source GPU data (must exist in repo or be placed here).
  - `btc_yearly.csv` — Yearly average Bitcoin price (`year`, `btc_avg_price`), fetched via yfinance.
  - `sp500_yearly.csv` — S&P 500 (`year`, `sp500_close`, `sp500_return`), fetched via yfinance.
  - `supply_chain_yearly.csv` — GPU supply chain: TSMC (TSM), NVIDIA (NVDA), AMD (AMD), Micron (MU) yearly close and return, fetched via yfinance. (Older `semiconductor_yearly.csv` with TSMC/Qualcomm is deprecated; re-run EDA to generate supply chain data.)

- **processed/** — Outputs from notebooks.
  - `gpu_clean.csv`, `gpu_yearly.csv` — From `eda_cleaning.ipynb`.
  - `merged_yearly.csv` — GPU yearly + BTC + SP500 + supply chain. From `correlation.ipynb`.

Run order: `eda_cleaning` → `correlation` → `regression` → `prediction` → `visualization`.
