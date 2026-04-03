# 📊 NIFTY Breakout Lab — Multi-Stock 5-Min Backtest System

A **production-ready, browser-based backtesting tool** for NSE 5-minute OHLCV data.  
Upload multiple stock CSVs → get full breakout analysis across all time windows — zero installation, runs entirely in the browser.

![Version](https://img.shields.io/badge/version-4.0--MULTI-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-NSE%20India-orange)
![Data](https://img.shields.io/badge/data-5--min%20OHLCV-yellow)

---

## 🚀 Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/nifty-breakout-lab.git
cd nifty-breakout-lab

# 2. Open the tool — NO installation needed
#    Just open the HTML file in any modern browser:
open nifty_backtest_multifile.html       # macOS
start nifty_backtest_multifile.html      # Windows
xdg-open nifty_backtest_multifile.html  # Linux

# 3. Drop your CSV files from data/ and click RUN
```

> 💡 **No Python, no server, no pip install** — it's a single self-contained HTML file.

---

## 📁 Project Structure

```
nifty-breakout-lab/
│
├── nifty_backtest_multifile.html   ← 🎯 Main tool (open this in browser)
│
├── data/
│   ├── README.md                   ← How to get & format data
│   └── sample/
│       └── LICI_5minute_sample.csv ← Demo file (20 rows, shows format)
│
├── .gitignore                      ← Excludes large CSV files from Git
└── README.md                       ← This file
```

> **Your actual CSV data files** (`NIFTY50_5minute.csv`, `TCS_5minute.csv`, etc.) go in the `data/` folder but are **excluded from Git** via `.gitignore` (they are 10–11 MB each). See [`data/README.md`](data/README.md) for how to obtain them.

---

## 🎯 What It Does

### Strategy: First Breakout of Range
For each trading day, the engine:
1. Builds a **price range** using all bars up to a configurable time window (e.g. 10:15 AM)
2. Enters **LONG** on the first bar that breaks above the range high
3. Enters **SHORT** on the first bar that breaks below the range low
4. Exits after a fixed hold duration (default: 20 min)
5. Optionally uses the opposite side of the range as Stop Loss

This is tested across **10 time windows** simultaneously:
- **AM windows:** 09:30, 09:45, 10:00, 10:15, 10:30
- **PM windows:** 14:00, 14:15, 14:30, 14:45, 15:00

---

## 📊 Dashboard Views

### 1. Stock Summary Tab
The flagship view — **one row per stock** showing:

| Column | Description |
|--------|-------------|
| **STOCK** | Name (auto-detected from filename) + date range |
| **TOTAL TRADES** | Total breakout signals across all windows |
| **OVERALL WIN RATE** | Combined win% across all 10 windows |
| **TOTAL POINTS (ALL)** | Gross PnL sum across all windows |
| **BEST WINDOW** | Highest-points window + its pts |
| **TOP 3 BY POINTS** | Top 3 windows with individual points |
| **BEST WIN RATE** | Window with highest win%, its % |
| **WEAKEST WINDOW** | Worst window — consider skipping |

Fully **sortable** (click any column), **searchable** (type stock name), with stock-wise color coding.

### 2. Compare Charts Tab
- Total points per stock (bar chart)
- Win rate: overall vs best window (overlay)
- Cross-stock window breakdown (all 10 windows × all stocks)
- Full comparison table with every window × every stock

### 3. Stock Detail Tab
Click any row in the summary to drill into a full per-stock view:
- KPI row (trades, WR, total pts, best window, price range)
- Strategy insights (top 3 windows, AM vs PM edge, best WR, weakest window)
- AM + PM window tables (n, WR%, avg PnL, avg win, avg loss, W/L ratio, total pts)
- Cumulative equity curve (per window or combined)
- Year-wise P&L table (best 4 windows × each year)

---

## ⚙️ Configuration

All parameters are set in the UI before running — no code changes needed:

| Parameter | Default | Description |
|-----------|---------|-------------|
| Hold Duration | 20 min | How long to hold after breakout entry |
| Min Range | 5 pts | Minimum range size to qualify a window |
| Session | AM + PM | Filter to AM-only, PM-only, or both |
| SL Mode | None | No SL, or use opposite range boundary |

---

## 📂 Data Requirements

**Format:** NSE 5-minute OHLCV CSV

```csv
date,open,high,low,close,volume
2022-05-17 09:15:00,872.0,875.0,870.0,873.5,3759243
2022-05-17 09:20:00,873.5,876.0,872.0,875.0,1250000
```

**Compatible sources:**
- Zerodha KiteConnect `historical_data()` API
- Angel One SmartAPI historical endpoint
- Upstox / Fyers / Shoonya / Dhan historical APIs
- Any broker providing NSE 5-min OHLCV

See [`data/README.md`](data/README.md) for detailed data fetching instructions with code.

**Tested stocks** (from `C:\Raj\app\NiftyData\10Y_5M_2015_2025\`):
```
NIFTY 50 · NIFTY BANK · TCS · LICI · HDFCBANK · MARUTI
HEROMOTOCO · LT · ULTRACEMCO · BRITANNIA · KOTAKBANK
APOLLOHOSP · ASIANPAINT · HINDUNILVR · DIVISLAB
INDUSINDBK · INFY · SHREECEM · BOSCHLTD · and more
```

---

## 🔬 Backtest Engine Details

- **Bar-by-bar simulation** — no look-ahead bias
- **Chunked async processing** — browser stays responsive during computation
- **Robust CSV parser** — handles datetime, date, timestamp column names; multiple date formats; Windows/Linux line endings
- **Session labels** — S1/S2/S3 anchors at 11:27, 13:27, 15:30 for day-type classification (TREND_UP, TREND_DOWN, LATE_REV_UP, etc.)
- **No external dependencies** — Chart.js loaded from CDN, everything else is vanilla JS

---

## 🗂️ Related Projects

This tool is part of a broader **Algorithmic Trading Suite** built for NSE markets:

| Project | Description | Stack |
|---------|-------------|-------|
| **Breakout Lab** ← You are here | Multi-stock 5-min breakout backtester | HTML + JS |
| **ML-VWAP Dashboard** | VWAP + Gap + ML (XGBoost/RF/LSTM) research paper implementations | Python + Streamlit |
| **ORB+FVG Strategy** | Opening Range Breakout + Fair Value Gap with 5 modes | Python + Zerodha KiteConnect |
| **Smart Flow Confluence** | 8-module confluence system (VWAP, RSI, PCR, IV, India VIX) | Python |
| **Agentic FnO Trader** | CrewAI-based automated FnO trading | Python + CrewAI |
| **RBI Bot v3** | Production trading bot — MACD, RSI MeanRev, CVD strategies | Python + Rich dashboard |

---

## 📈 Sample Results (LICI, 2022–2025)

> Results are **gross points only** — no commissions, no slippage, no lot size applied.  
> Always apply realistic transaction costs before trading live.

| Window | Trades | Win Rate | Total Pts |
|--------|--------|----------|-----------|
| AM 10:15 | ~1,200 | ~52–55% | Varies by stock |
| PM 14:15 | ~1,100 | ~51–54% | Varies by stock |

---

## 🚧 Limitations & Disclaimer

- **Gross PnL only** — No commissions, STT, slippage, or impact cost deducted
- **No position sizing** — Each trade = 1 unit (apply lot size externally)
- **NSE RTH only** — 09:15 to 15:30 IST, Monday–Friday
- **Past performance ≠ future results**
- This is a research/education tool — not financial advice

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| UI | Vanilla HTML5 + CSS3 (CSS variables, grid, flexbox) |
| Charts | Chart.js 4.4.1 (CDN) |
| Engine | Vanilla JavaScript (ES2020, async/await, chunked processing) |
| Fonts | IBM Plex Mono + Bebas Neue + Barlow (Google Fonts) |
| Data | Browser FileReader API — no server needed |

---

## 📝 License

MIT License — free to use, modify, and distribute.  
Attribution appreciated but not required.

---

## 🤝 Contributing

Pull requests welcome. Key areas:
- Additional exit strategies (trailing SL, target-based)
- Monthly P&L breakdown view
- CSV export of trade log per stock
- Angel One / Upstox data fetch script in `data/`

---

*Built for Indian NSE markets · 5-minute OHLCV · Breakout research*
