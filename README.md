# Monte Carlo Pricing for American Call Options with Dividends

A comprehensive Jupyter notebook implementing **Monte Carlo pricing for American-style call options** using real US market data (AAPL).

## Features

- **Geometric Brownian Motion (GBM)** with dividends
- **Longstaff-Schwartz Monte Carlo (LSMC)** for early exercise decisions
- **Black-Scholes-Merton (BSM)** for European call approximation
- **Implied Volatility Smile** from real option chain data
- **Real-time data** via yfinance (AAPL)
- **Comprehensive visualizations**: GBM paths, payoff distributions, convergence analysis

## Quick Start

### 1. Install Dependencies

```bash
cd /Users/user/Documents/Quant-Finance-Project1
source .venv/bin/activate
pip install -r requirements.txt
```

### 2. Run in Cursor

1. Open `american_call_mc_pricing.ipynb` in Cursor
2. Select kernel: **".venv/bin/python"** (top-right)
3. Run cells sequentially: `Shift+Enter` for each cell
4. Or use "Run All" (takes ~5-7 minutes)

### 3. Run in Browser (Alternative)

```bash
source .venv/bin/activate
jupyter notebook
```

Then open `american_call_mc_pricing.ipynb` in the browser.

## Notebook Structure

1. **Fetch US Market Data** - Real AAPL data (S0, σ, q, r)
2. **Simulate GBM Paths** - Stock price paths with dividends
3. **BSM European Call** - Theoretical benchmark price
4. **LSMC American Call** - Monte Carlo with early exercise
5. **Convergence Analysis** - Price vs number of paths
6. **Implied Volatility Modeling** - IV smile from option chain
7. **Visualizations** - 3 stacked plots (paths, payoffs, convergence)
8. **Interactive Widgets** - Adjust volatility slider
9. **Final Summary** - Complete statistics

## Expected Output

- **BSM European Call**: ~$15-25 (depends on market conditions)
- **MC American Call**: ~$15-26 (slightly higher due to early exercise)
- **Visualizations**: 3 plots stacked vertically with dark background

## Requirements

- Python 3.8+
- numpy, matplotlib, scipy, yfinance, ipywidgets, pandas
- Jupyter notebook or Cursor with Jupyter extension

## Notes

- Uses real AAPL data from yfinance
- Convergence analysis uses up to 25,000 paths (reduced from 100k for speed)
- Dividend yield significantly affects call option prices
- All plots use dark background theme

## Files

- `american_call_mc_pricing.ipynb` - Main notebook
- `requirements.txt` - Python dependencies
- `README.md` - This file

## License

Educational use.

