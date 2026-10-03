# T58 Market Data

Historical market data for the [T58 Quant Algo Backtester](https://github.com/OWNER/T58-QUANT-ALGO-BACKTESTER). This repo holds data only, with no code, so the backtester repo stays small.

> **Private use only.** See [LICENSE](LICENSE). Some of this data comes from third-party vendors and may not be redistributed.

## Instruments (29)

One folder per instrument. Folder names match the table exactly.

<!-- Fill in timeframes and date ranges once, here: e.g. "All folders: 1m and 5m bars, YYYY-MM-DD to YYYY-MM-DD" -->

### Futures (continuous front-month, `1!`)

| Folder | Instrument |
|--------|------------|
| `ES1!` | E-mini S&P 500 |
| `MES1!` | Micro E-mini S&P 500 |
| `NQ1!` | E-mini Nasdaq-100 |
| `MNQ1!` | Micro E-mini Nasdaq-100 |
| `GC1!` | Gold |
| `MGC1!` | Micro Gold |
| `SI1!` | Silver |

### Indices

| Folder | Instrument |
|--------|------------|
| `S&P 500` | S&P 500 |
| `NASDAQ 100` | Nasdaq-100 |
| `DOW JONES 30` | Dow Jones Industrial Average |
| `RUSSELL 2000` | Russell 2000 |

### Forex

| Folder | Pair |
|--------|------|
| `AUDUSD` | Australian dollar / US dollar |
| `EURUSD` | Euro / US dollar |
| `GBPUSD` | British pound / US dollar |
| `NZDUSD` | New Zealand dollar / US dollar |
| `USDCAD` | US dollar / Canadian dollar |
| `USDCHF` | US dollar / Swiss franc |
| `USDJPY` | US dollar / Japanese yen |

### Metals (spot)

| Folder | Instrument |
|--------|------------|
| `XAUUSD (GOLD)` | Gold |
| `XAGUSD (SILVER)` | Silver |
| `XCUUSD (COPPER)` | Copper |
| `XPTUSD (PLATINUM)` | Platinum |

### Energy

| Folder | Instrument |
|--------|------------|
| `BRENT CRUDE OIL` | Brent crude oil |

### Crypto

| Folder | Instrument |
|--------|------------|
| `BITCOIN` | Bitcoin |
| `ETHEREUM` | Ethereum |
| `SOLANA` | Solana |
| `DOGECOIN` | Dogecoin |

### Stocks

| Folder | Instrument |
|--------|------------|
| `NVIDIA` | NVIDIA |
| `YAHOO` | <!-- describe: Yahoo stock, or data pulled from Yahoo Finance? --> |

## Layout

```
<INSTRUMENT FOLDER>/
  <files, e.g. SYMBOL_TIMEFRAME.csv or .parquet>
README.md
LICENSE
```

Name files `SYMBOL_TIMEFRAME.ext` (e.g. `ES1_5m.parquet`) so they are easy to find.

## Format

Columns, in this order:

| Column | Type | Notes |
|--------|------|-------|
| `timestamp` | datetime | Bar **open** time. Timezone: `<UTC / America/New_York / ...>` |
| `open` | float | |
| `high` | float | |
| `low` | float | |
| `close` | float | |
| `volume` | float | Contracts / units traded in the bar. Forex volume may be tick volume |

- Sorted ascending by `timestamp`, no duplicate timestamps.
- Futures (`1!`) are continuous contracts: `<describe roll / adjustment method>`.
- Missing bars (weekends, holidays, maintenance halts) are left out, not filled.

## Using it with the backtester

1. Clone this repo (or download a single file).
2. In the backtester, open the **Market Data** page and upload the file.
3. Pick the matching instrument so point value and tick size are applied correctly.

Quick check in Python (quote the path, since folder names contain `!`, spaces and `&`):

```python
import pandas as pd

df = pd.read_csv("ES1!/ES1_5m.csv", parse_dates=["timestamp"])  # or pd.read_parquet(...)
print(df.head())
print(df["timestamp"].is_monotonic_increasing)
```

## Sources

<!-- Be specific. You will want this later when you check what you are allowed to do with each file. -->

| Data | Source | Terms |
|------|--------|-------|
| All folders | `<vendor / broker / API>` | `<link to terms>` |

## Known limitations

- Bar data only. No tick data, no order book.
- Backtests on this data do not model slippage or liquidity. Set those in the backtester.
- Spot forex and metals have no central exchange, so prices differ slightly between vendors.
- Not guaranteed free of gaps, bad prints or vendor errors. Run the backtester's integrity check before trusting a result.

## Adding or updating data

1. Add a folder (or files in an existing one) using the naming convention above.
2. Add the instrument to the right table.
3. Commit with a message like `Add USDMXN 5m 2020-2026`.

**File size:** GitHub blocks files over 100 MB. Prefer Parquet (far smaller than CSV). If a file is still too big, use [Git LFS](https://git-lfs.com) (`git lfs track "**/*.parquet"`).

## Disclaimer

Data is provided as is, with no warranty of accuracy or completeness. Nothing here is financial advice. Past performance does not predict future results.
