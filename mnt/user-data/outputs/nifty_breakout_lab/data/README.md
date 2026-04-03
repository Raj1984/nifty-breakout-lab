# 📁 Data Folder

This folder holds all your **NSE 5-minute OHLCV CSV files** for backtesting.

## Folder Layout

```
data/
├── README.md                   ← This file
├── sample/
│   └── LICI_5minute_sample.csv ← Demo file (20 rows) — shows required format
└── (your files go here)
    ├── NIFTY 50_5minutedata.csv
    ├── NIFTY BANK_5minutedata.csv
    ├── TCS_5minute.csv
    ├── HDFCBANK_5minute.csv
    ├── LICI_5minute.csv
    └── ... any NSE stock
```

> **Note:** The actual data files (10+ MB each) are NOT committed to Git.  
> Only the `sample/` folder with a tiny demo CSV is tracked.  
> See below for how to get the real data.

---

## Required CSV Format

```
date,open,high,low,close,volume
2022-05-17 09:15:00,872.0,875.0,870.0,873.5,3759243
2022-05-17 09:20:00,873.5,876.0,872.0,875.0,1250000
```

| Column | Required | Notes |
|--------|----------|-------|
| `date` | ✅ | `YYYY-MM-DD HH:MM:SS` format, IST |
| `open` | ✅ | Bar open price |
| `high` | ✅ | Bar high price |
| `low`  | ✅ | Bar low price |
| `close`| ✅ | Bar close price |
| `volume`| ❌ | Optional — used for info only |

The tool auto-detects column names (case-insensitive, also handles `datetime`, `ltp`, etc.)

---

## How to Get NSE 5-Minute Data

### Option 1 — Zerodha KiteConnect (Recommended)
```python
from kiteconnect import KiteConnect
import pandas as pd

kite = KiteConnect(api_key="YOUR_KEY")
kite.set_access_token("YOUR_TOKEN")  # refresh daily

# NIFTY 50 token = 256265, NIFTY BANK = 260105
hist = kite.historical_data(
    instrument_token=256265,
    from_date="2015-01-01",
    to_date="2025-12-31",
    interval="5minute"
)
df = pd.DataFrame(hist)
df.rename(columns={"date":"date","open":"open","high":"high",
                   "low":"low","close":"close","volume":"volume"}, inplace=True)
df.to_csv("data/NIFTY 50_5minutedata.csv", index=False)
```

### Option 2 — Angel One SmartAPI
```python
from smartapi import SmartConnect
# Similar pattern — use symboltoken for the instrument
```

### Option 3 — Upstox / Fyers / Shoonya
Most brokers with API access provide historical 5-min data via their SDK.

---

## Naming Convention

The tool extracts the stock name from the filename by stripping common suffixes:

| Filename | Detected as |
|----------|-------------|
| `NIFTY 50_5minutedata.csv` | `NIFTY 50` |
| `TCS_5minute.csv` | `TCS` |
| `HDFCBANK_5minute.csv` | `HDFCBANK` |
| `LICI_5minute.csv` | `LICI` |

Any `.csv` filename works — just be consistent.
