# Sentiment-to-Signal

A quantitative research pipeline that builds tradeable ML signals from alternative data, validated on equity and crypto markets.

> 🚧 **Status:** In active development (June – August 2026). Project scaffolding and data ingestion in progress. See roadmap below.

---

## Motivation

Most retail-accessible "alpha" research suffers from three problems: random train/test splits that leak future information into model training, ignored or unrealistic transaction costs, and a single asset class that hides regime sensitivity. This project addresses all three.

The headline question: **does sentiment extracted from social media predict next-day equity returns after controlling for market beta and sector momentum, net of realistic execution costs?**

A secondary question applies the same framework to crypto: **does combining short-horizon momentum with perpetual futures funding rates produce a market-neutral strategy that beats buy-and-hold BTC on a risk-adjusted basis?**

## Approach

### Strategy 1 — Reddit sentiment on US equities

- **Universe:** Top 200 most-mentioned tickers across r/wallstreetbets, r/stocks, r/investing
- **Period:** 2018–2023, daily frequency
- **Sentiment model:** FinBERT (`ProsusAI/finbert`)
- **Features:** Per ticker per day — post count, average sentiment, sentiment dispersion, sentiment momentum, plus controls (prior return, volatility, sector ETF return, market beta)
- **Models:** Logistic regression (baseline), XGBoost (main), with ablation studies isolating sentiment's incremental signal
- **Validation:** Walk-forward expanding-window CV, refit annually
- **Backtest:** Daily rebalance, equal-weighted top-decile long / bottom-decile short, sector-neutral, 5 bps fees + 10 bps slippage

### Strategy 2 — Crypto momentum + funding rate

- **Universe:** Top 20 perpetual futures on Binance
- **Period:** 2022–2025, hourly frequency
- **Signal:** 24h momentum × normalized funding rate
- **Backtest:** Vol-targeted position sizing (15% annualized portfolio vol), 4 bps taker fees + 5 bps slippage

## Repository structure

```
sentiment-to-signal/
├── core/                          # Shared infrastructure
│   ├── data_loader.py             # Pulls and caches data
... (etc)
└── README.md
```

## Roadmap

- [ ] Project scaffolding and environment setup
- [ ] Core data loaders (Reddit, equity, crypto)
- [ ] FinBERT sentiment extraction pipeline
- [ ] Feature engineering at ticker-day level
- [ ] Walk-forward CV framework
- [ ] Baseline + XGBoost models with ablation studies
- [ ] Equity backtest with realistic execution costs
- [ ] Crypto strategy build and backtest
- [ ] Performance attribution and regime analysis
- [ ] Final writeup with results and self-critique

## Tech stack

`Python` `pandas` `numpy` `scikit-learn` `XGBoost` `transformers` (FinBERT) `praw` `yfinance` `ccxt` `matplotlib` `pytest`

## A note on results

This project is being run with the assumption that **honest negative or modest results are more valuable than cherry-picked positive ones.** The headline number reported here will be the post-cost, out-of-sample Sharpe ratio, with full disclosure of methodology, costs, and limitations. If sentiment doesn't beat controls, the README will say so.

---

**Author:** [Adrian Lin](https://github.com/adrianlin1) · adrian1@bu.edu
