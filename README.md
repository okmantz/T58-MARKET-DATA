# T58 Market Data

Historical market data for the [T58 Quant Algo Backtester](https://github.com/OWNER/T58-QUANT-ALGO-BACKTESTER). This repo holds data only, with no code, so the backtester repo stays small.

> **Private use only.** See [LICENSE](LICENSE). Some of this data comes from third-party vendors and may not be redistributed.

## What's in here

<!-- Fill in / delete rows to match the repo -->

| Symbol | Instrument | Timeframe | Range | File |
|--------|------------|-----------|-------|------|
| ES | E-mini S&P 500 futures | 1m | YYYY-MM-DD to YYYY-MM-DD | `data/ES_1m.parquet` |
| MGC | Micro Gold futures | 1m | YYYY-MM-DD to YYYY-MM-DD | `data/MGC_1m.parquet` |

## Layout

```
data/
  <SYMBOL>_<TIMEFRAME>.<csv|parquet>
README.md
LICENSE
```

One file per symbol and timeframe. Name new files `SYMBOL_TIMEFRAME.ext` (e.g. `NQ_5m.parquet`) so they are easy to find and the backtester can pick the right one.

## Format

Columns, in this order:

| Column | Type | Notes |
|--------|------|-------|
| `timestamp` | datetime | Bar **open** time. Timezone: `<UTC / America/New_York / ...>` |
| `open` | float | |
| `high` | float | |
| `low` | float | |
| `close` | float | |
| `volume` | float | Contracts traded in the bar |

- Sorted ascending by `timestamp`, no duplicate timestamps.
- Prices are `<raw / back-adjusted / continuous contract method>`.
- Missing bars (weekends, holidays, maintenance halts) are left out, not filled.

## Using it with the backtester

1. Clone this repo (or download a single file).
2. In the backtester, open the **Market Data** page and upload the file.
3. Pick the matching instrument so point value and tick size are applied correctly.

Quick check in Python:

```python
import pandas as pd

df = pd.read_parquet("data/ES_1m.parquet")   # or pd.read_csv(..., parse_dates=["timestamp"])
print(df.head())
print(df.index.is_monotonic_increasing, df["timestamp"].is_monotonic_increasing)
```

## Sources

<!-- Be specific. You will want this later when you check what you are allowed to do with each file. -->

| Data | Source | Terms |
|------|--------|-------|
| ES, MGC | `<vendor / broker / API>` | `<link to terms>` |

## Known limitations

- Bar data only. No tick data, no order book.
- Backtests on this data do not model slippage or liquidity. Set those in the backtester.
- Contract rolls: `<describe how the continuous series was built>`.
- Not guaranteed free of gaps, bad prints or vendor errors. Run the backtester's integrity check before trusting a result.

## Adding or updating data

1. Add the file under `data/` using the naming convention above.
2. Add a row to the table at the top and to Sources.
3. Commit with a message like `Add NQ 5m 2020-2026`.

**File size:** GitHub blocks files over 100 MB. Prefer Parquet (far smaller than CSV). If a file is still too big, use [Git LFS](https://git-lfs.com) (`git lfs track "data/*.parquet"`).

## Disclaimer

Data is provided as is, with no warranty of accuracy or completeness. Nothing here is financial advice. Past performance does not predict future results.
