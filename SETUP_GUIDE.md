# Setup Guide for American Call MC Pricing Notebook

## Quick Start

1. **Activate your virtual environment** (if using one):
   ```bash
   source .venv/bin/activate  # On Mac/Linux
   # or
   .venv\Scripts\activate  # On Windows
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch Jupyter**:
   ```bash
   jupyter notebook
   ```
   Or if using JupyterLab:
   ```bash
   jupyter lab
   ```

4. **Open the notebook**: `american_call_mc_pricing.ipynb`

5. **Run all cells**: Go to `Cell` → `Run All` (or press `Shift+Enter` for each cell)

## What Each Section Does

### Section 1: Fetch US Market Data
- Downloads real AAPL stock data using yfinance
- Calculates current price (S0), historical volatility (σ), and dividend yield (q)
- Sets up option parameters (strike K, time T, steps)

### Section 2: Simulate GBM Paths
- Creates function to generate stock price paths using Geometric Brownian Motion
- Accounts for dividends in the model
- Uses exact mathematical solution for efficiency

### Section 3: BSM European Call
- Calculates theoretical European call option price
- Uses Black-Scholes-Merton formula with dividends
- This is our benchmark for comparison

### Section 4: LSMC American Call
- Implements Longstaff-Schwartz Monte Carlo algorithm
- Handles early exercise decisions (American-style)
- Uses regression to estimate continuation values

### Section 5: Convergence Analysis
- Tests different numbers of simulation paths (10 to 100,000)
- Shows how price estimate improves with more paths
- Calculates confidence intervals

### Section 6: Implied Volatility Modeling
- Fetches real option chain data from market
- Calculates implied volatility for different strikes
- Creates "volatility smile" visualization

### Section 7: Comprehensive Visualizations
- Creates 4-panel plot showing:
  - Stock price paths
  - Payoff distribution
  - Convergence analysis
  - Volatility smile

### Section 8: Interactive Visualization
- Interactive widget to adjust volatility
- See how paths change in real-time

### Section 9: Final Summary
- Prints all key statistics
- Compares American vs European prices
- Shows IV statistics

## Why Prices Might Be Smaller Than Expected

1. **Dividend Yield (q)**: When q > 0, the stock price grows slower, reducing option value
2. **At-the-Money Strike**: We use K = round(S0), which is ATM - these have lower intrinsic value
3. **Early Exercise**: For American calls with dividends, early exercise can reduce value
4. **Time Value**: With 1 year to expiration, there's time value, but dividends reduce it
5. **Volatility**: Historical volatility might be lower than expected

## Troubleshooting

- **Import errors**: Make sure all packages are installed
- **Data fetch errors**: Check internet connection (yfinance needs web access)
- **Slow execution**: The convergence analysis with 100k paths takes time - be patient!
- **No option chain data**: The notebook will use hypothetical data if real data unavailable

