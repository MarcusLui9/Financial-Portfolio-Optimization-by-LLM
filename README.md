# 📈 Financial Portfolio Optimization by LLM

**Author:** Marcus Lui | Claude Assisted  
**Date:** June 2026

---

## Overview

This project investigates whether a large language model (Claude) can be used for professional-grade financial portfolio optimization. It covers two core questions:

1. Can an LLM perform financial optimization within user-defined constraints and guardrails?
2. Can Claude generate production-quality Python notebooks, executive summaries, and slide decks?

**Conclusion:** Yes to both — with a human review step before official use.

---

## Methodology

The optimizer uses a three-step pipeline:

| Step | Input | Method |
|------|-------|--------|
| Expected Returns (μ) | Analyst-supplied forward-looking views | Not historical means |
| Covariance Matrix (Σ) | Historical daily log returns (2021–present) | Annualized × 252 |
| Optimization | Maximize Sharpe Ratio | SLSQP solver, fully invested |

**Risk-Free Rate:** 5.00%  
**Benchmark:** Equal-weight (1/N) portfolio  
**Position Bounds:** Min 4% | Max 24% per ETF

---

## Asset Universe

| Ticker | Asset Class | Analyst μ | Hist. Volatility | Implied Sharpe |
|--------|-------------|-----------|-----------------|----------------|
| IWV | US Equity (Russell 3000) | 8.00% | ~14% | 0.214 |
| IEMG | Emerging Market Equity | 8.00% | ~17% | 0.176 |
| VEA | Developed Non-US Equity | 7.90% | ~14% | 0.207 |
| JNK | High-Yield Bond | 6.60% | ~9% | 0.178 |
| SPTL | Long-Term Government Bond | 5.10% | ~15% | 0.007 |
| VCIT | Investment-Grade Corp Bond | 4.90% | ~7% | -0.014 |
| TIP | Inflation-Protected Bond | 4.70% | ~8% | -0.038 |

*Implied Sharpe = (μ − Rf) / σ | Rf = 5.00%*

---

## Results

### Portfolio Performance

| Metric | Equal-Weight | Max-Sharpe Optimized | Δ Change |
|--------|-------------|----------------------|----------|
| Expected Return | 6.64% | 7.11% | +0.47% |
| Volatility | 9.32% | 8.82% | −0.50% |
| Sharpe Ratio | 0.178 | 0.238 | +0.060 |

**33.7% Sharpe improvement vs. equal-weight baseline** — achieved purely through weight reallocation, with no change to data inputs.

### Optimal Weights

| Ticker | Equal Wt. | Optimal Wt. | Δ vs Equal | Status |
|--------|-----------|-------------|------------|--------|
| IWV | 14.3% | 24.0% | +9.7% | MAX ▲ |
| IEMG | 14.3% | 24.0% | +9.7% | MAX ▲ |
| VEA | 14.3% | 24.0% | +9.7% | MAX ▲ |
| JNK | 14.3% | 10.0% | −4.3% | FREE |
| SPTL | 14.3% | 4.0% | −10.3% | MIN ▼ |
| VCIT | 14.3% | 10.0% | −4.3% | FREE |
| TIP | 14.3% | 4.0% | −10.3% | MIN ▼ |

All three equity ETFs hit the 24% maximum bound. SPTL and TIP were minimized to 4%, reflecting their poor marginal Sharpe contribution at the 5% risk-free rate.

### Efficient Frontier

| Portfolio | Return | Volatility | Sharpe |
|-----------|--------|------------|--------|
| Global Minimum Variance | 5.18% | 7.21% | 0.025 |
| Tangency (Max-Sharpe) | 7.11% | 8.82% | 0.238 |
| Equal-Weight (1/N) | 6.64% | 9.32% | 0.178 |

---

## Project Workflow

```
Step 1 → Hand-code Python notebook (SLSQP optimization)
Step 2 → Iteratively prompt Claude Code to replicate and cross-review
Step 3 → Prompt Claude to generate slide deck and executive summary
Step 4 → Human review and cleanup of all deliverables
```

### Claude Prompt Used for Notebook Generation

> *"Generate a notebook named ETF_Target_Weight_Optimization. Solve for optimal weights of a portfolio of a certain number of assets. The weights are bound between 4% and 24% of the portfolio. All weights must add up to 100%..."*

---

## Files

| File | Description |
|------|-------------|
| `Asset_Portfolio_Optimization.ipynb` | Hand-coded baseline notebook |
| `ETF_Target_Weight_Optimization.ipynb` | Claude Code-generated notebook |
| `ETF_Target_Weight_Optimization_v3.pdf` | Executive summary report |
| `LLM_Abilities_Project.pdf` | Project slide deck |

---

## Key Takeaways

- LLMs can execute financial optimization tasks (e.g., max-Sharpe weight solving) when given precise constraints and expected return inputs
- Claude-generated code was cross-validated against hand-written code and produced consistent results
- Generated documents and slide decks are near production-ready but require a final human-led review before official use
- The max-Sharpe optimizer converged in under 100 iterations using SLSQP

---

## Disclaimer

This project is for educational and research purposes only. All expected returns are analyst-supplied hypothetical forward-looking views and do not constitute investment advice.
