# Trader Performance vs Market Sentiment — Hyperliquid

---

## Setup

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

Place `historical_data.csv` and `fear_greed_index.csv` in the same directory, then:

```bash
jupyter notebook trader_sentiment_analysis.ipynb
```

---

## What's in here

- `trader_sentiment_analysis.ipynb` — main notebook (all analysis, charts, write-up)
- `fig1_performance.png` — PnL distribution, win rate, trade frequency, L/S ratio
- `fig2_segments.png` — trader segmentation by leverage and frequency
- `fig3_behavior.png` — behavioral shifts under Fear vs Greed
- `fig4_drawdown_cum.png` — drawdown proxy and cumulative PnL over time
- `README.md` — this file

---

## Dataset Overview

| Dataset | Rows | Cols | Notes |
|---|---|---|---|
| historical_data.csv | 211,224 | 16 | Hyperliquid trades, May 2023–May 2025 |
| fear_greed_index.csv | 2,644 | 4 | Daily BTC sentiment, Feb 2018–May 2025 |

No missing values or duplicates found in either dataset.

---

## Methodology

**Alignment:** Trader timestamps (IST) were parsed to date-level and merged with the Fear/Greed index on date. The 5-class sentiment (Extreme Fear, Fear, Neutral, Greed, Extreme Greed) was collapsed to a binary Fear/Greed label. Neutral days were excluded from comparative analysis.

**Closing trades:** PnL analysis was restricted to closing directions (Close Long, Close Short, Long>Short, Short>Long) to avoid double-counting open positions.

**Segments:** Traders were split into terciles by average trade size (leverage proxy) and total trade count (frequency proxy).

**Drawdown proxy:** Computed as the minimum value of (cumulative PnL − rolling max of cumulative PnL) per trader per sentiment regime.

---

## Key Insights

**1. Fear days are more profitable than Greed days**

Average daily PnL on Fear days was $39,012 vs $15,848 on Greed days. Win rate was also higher — 86.3% vs 80.8%. This is somewhat counterintuitive but makes sense given the trader composition: disciplined accounts who stay active during fear periods capture better entries.

**2. Trading activity spikes sharply during Fear**

Traders execute 792 trades/day during Fear vs just 294 during Greed — nearly 3× higher. This suggests these Hyperliquid accounts are mean-reversion or dip-buying oriented rather than trend-following. They step in hard when the market is fearful.

**3. Greed days produce deeper drawdowns despite lower activity**

Average max drawdown during Greed days was -$30,758 vs -$20,266 during Fear. This means that when traders do trade in bull markets, they tend to oversize or hold longer, resulting in larger adverse excursions when the trade goes against them.

---

## Strategy Recommendations

**Strategy 1 — Lean in on Fear days (for high-frequency consistent traders)**

The data consistently shows better outcomes during Fear. Rule: don't reduce activity when the market panics — that's when this trader cohort performs best. Keep position sizes moderate (don't chase size just because sentiment is extreme), but maintain frequency. Stop-loss discipline matters more here because drawdowns, while lower on average, can still be severe for individual traders.

**Strategy 2 — Cut size on Greed days for high-leverage accounts**

High-leverage traders see the worst risk-adjusted outcomes during Greed. The typical pattern is lower volume but higher drawdown, suggesting these traders are fighting the trend. Rule: during Greed regimes, high-leverage segment should scale back position size by 20–30% and tighten exits. Low-leverage, infrequent traders can stay the course — their behavior seems more suited to trending markets.

