# 🚀 Quick Start Guide

## Step 1: Install Dependencies (if not done)
```bash
cd /Users/user/Documents/Quant-Finance-Project1
source .venv/bin/activate
pip install -r requirements.txt
```

## Step 2: Launch Jupyter
```bash
jupyter notebook
```
This will open in your browser automatically.

## Step 3: Open the Notebook
Click on `american_call_mc_pricing.ipynb`

## Step 4: Run All Cells
- Go to menu: **Cell → Run All**
- OR press `Shift+Enter` in each cell sequentially

## ⏱️ Expected Runtime
- Sections 1-4: ~10-30 seconds
- Section 5 (Convergence): **5-10 minutes** (100k paths takes time!)
- Sections 6-9: ~30-60 seconds

**Total: ~6-12 minutes** (mostly waiting for convergence analysis)

---

## 📊 What You'll See

### Section 1 Output:
```
Fetching AAPL market data...
Current Stock Price (S0): $180.50
Historical Volatility (σ): 0.2800 (28.00%)
Dividend Yield (q): 0.0045 (0.45%)
Risk-free Rate (r): 0.0500 (5.00%)
```

### Section 5 Output (Convergence):
```
Running convergence analysis...
N=    10: MC Price = $XX.XXXX ± $X.XXXX (95% CI)
N=    50: MC Price = $XX.XXXX ± $X.XXXX (95% CI)
...
N=100000: MC Price = $XX.XXXX ± $X.XXXX (95% CI)
```

### Section 9 Output (Summary):
```
======================================================================
MONTE CARLO PRICING SUMMARY
======================================================================

Market Data:
  Stock: AAPL
  Current Price (S0): $180.50
  Strike (K): $181.00
  ...
  
Option Prices:
  MC American Call: $XX.XXXX ± $X.XXXX (95% CI)
  BSM European Call: $XX.XXXX
  Difference: $X.XXXX
```

---

## 🎨 Visualizations

You'll see:
1. **Colorful GBM paths** (100 paths)
2. **Payoff histogram** (distribution of outcomes)
3. **Convergence plot** (price vs number of paths)
4. **Volatility smile** (IV vs strike price)

All on dark background for easy viewing!

---

## ❓ Why Prices Are Smaller

**Main reasons:**
1. **Dividend yield (q)** - Dividends reduce call option value
2. **ATM strike** - At-the-money has no intrinsic value
3. **Historical volatility** - Might be lower than expected

**To see higher prices, try:**
- Change `K = S0 * 0.9` (ITM strike)
- Or `q = 0.0` (no dividends)

---

## 🆘 Troubleshooting

**"Module not found" error:**
```bash
pip install -r requirements.txt
```

**"Can't fetch data" error:**
- Check internet connection
- yfinance might be rate-limited (wait a minute)

**"Kernel died" error:**
- Too many paths? Reduce: `N_paths_list = [10, 50, 100, 500, 1000, 5000]`

**Slow execution:**
- Normal! 100k paths takes time
- Be patient or reduce max paths

---

## 📚 Learn More

- See `NOTEBOOK_EXPLANATION.md` for detailed explanations
- See `SETUP_GUIDE.md` for setup details

Happy pricing! 🎯

