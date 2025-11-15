# Detailed Explanation: American Call MC Pricing Notebook

## 🎯 Overview

This notebook prices **American-style call options** using Monte Carlo simulation. American options can be exercised at any time before expiration, making them more complex (and potentially more valuable) than European options.

---

## 📊 Section-by-Section Breakdown

### **Section 1: Fetch US Market Data**

**What it does:**
- Connects to Yahoo Finance API via `yfinance`
- Downloads real AAPL (Apple) stock data
- Calculates key parameters:
  - **S0**: Current stock price (e.g., ~$180)
  - **σ (sigma)**: Historical volatility from past year's price movements
  - **q**: Dividend yield (AAPL pays dividends, reducing option value)
  - **r**: Risk-free rate (5% - hardcoded, typical for US Treasury)

**Why it matters:**
- Real data makes the analysis realistic
- Dividend yield is crucial - it affects option pricing significantly

**Output example:**
```
Current Stock Price (S0): $180.50
Historical Volatility (σ): 0.2800 (28.00%)
Dividend Yield (q): 0.0045 (0.45%)
```

---

### **Section 2: Simulate GBM Paths with Dividends**

**What it does:**
- Implements **Geometric Brownian Motion (GBM)** - the standard model for stock prices
- Formula: `dS = (r - q) S dt + σ S dW`
  - `(r - q)`: Net drift (risk-free rate minus dividends)
  - `σ`: Volatility (how much prices fluctuate)
  - `dW`: Random Brownian motion (the "random walk")

**The exact solution:**
```
S_t = S_{t-1} × exp((r - q - 0.5×σ²)×dt + σ×√dt×Z)
```
- This is mathematically exact (no approximation errors)
- Very fast to compute

**Why dividends matter:**
- Dividends reduce stock price growth
- For call options, this reduces value (you miss dividends if you exercise early)

---

### **Section 3: Black-Scholes-Merton European Call**

**What it does:**
- Calculates the **theoretical price** of a European call option
- European = can only exercise at expiration (simpler than American)
- Uses the famous Black-Scholes formula with dividends

**The formula:**
```
C = S₀×e^(-qT)×N(d₁) - K×e^(-rT)×N(d₂)
```
Where:
- `N()` = cumulative normal distribution
- `d₁, d₂` = calculated from stock price, strike, time, volatility

**Why we need it:**
- Benchmark to compare against Monte Carlo
- If MC ≈ BSM, our simulation is working correctly
- For American calls, MC should be ≥ BSM (early exercise premium)

---

### **Section 4: Longstaff-Schwartz Monte Carlo (LSMC)**

**What it does:**
- This is the **core algorithm** for pricing American options
- Monte Carlo = simulate thousands of possible stock price paths
- At each time step, decide: **exercise now** or **wait**?

**How it works:**
1. **Forward simulation**: Generate many stock price paths
2. **Backward induction**: Start at expiration, work backwards
3. **Regression**: At each step, use regression to estimate "continuation value"
   - If immediate exercise > continuation value → exercise
   - Otherwise → wait

**Basis functions (Laguerre polynomials):**
- Used in regression to estimate continuation value
- Degree 3 = uses 4 polynomial terms
- More sophisticated than simple linear regression

**Why it's complex:**
- Must consider all possible future paths
- Early exercise decision depends on future expectations
- This is why American options are harder to price!

---

### **Section 5: Convergence Analysis**

**What it does:**
- Runs Monte Carlo with different numbers of paths: [10, 50, 100, 500, 1k, 5k, 10k, 50k, 100k]
- Shows how price estimate improves with more simulations
- Calculates 95% confidence intervals

**Why it matters:**
- More paths = more accurate (but slower)
- Shows convergence to "true" price
- Confidence intervals show uncertainty

**What to expect:**
- Small N (10-100): Large variance, unstable
- Medium N (1k-10k): Stabilizing
- Large N (50k-100k): Converged, tight confidence intervals

**Time warning:**
- 100k paths takes several minutes!
- The notebook shows progress for each N

---

### **Section 6: Implied Volatility Modeling**

**What it does:**
- Fetches **real option chain data** from market
- For each strike price, calculates **implied volatility (IV)**
- IV = the volatility that makes BSM price = market price

**The volatility smile:**
- Plot of IV vs Strike price
- Usually shows a "smile" or "skew"
- OTM (out-of-the-money) options often have higher IV

**Why it's interesting:**
- Shows market expectations
- Real IV often differs from historical volatility
- Reveals market sentiment (fear, uncertainty)

**Fallback:**
- If option chain unavailable, generates hypothetical smile
- Still demonstrates the concept

---

### **Section 7: Comprehensive Visualizations**

**Four-panel plot:**

1. **Top Left: GBM Paths**
   - 100 colorful stock price simulations
   - Shows randomness and volatility
   - Red line = strike price, cyan = initial price

2. **Top Right: Payoff Histogram**
   - Distribution of option payoffs
   - Yellow line = MC price, orange = BSM price
   - Shows uncertainty in outcomes

3. **Bottom Left: Convergence Plot**
   - Price vs number of paths (log scale)
   - Shows convergence to stable value
   - Orange line = BSM benchmark
   - Shaded area = 95% confidence interval

4. **Bottom Right: Volatility Smile**
   - IV vs Strike price
   - Shows market's volatility expectations
   - Yellow line = current stock price

---

### **Section 8: Interactive Visualization**

**What it does:**
- Interactive slider to adjust volatility (σ)
- See how paths change in real-time
- Great for understanding volatility's impact

**How to use:**
- Move the slider
- Watch paths become more/less volatile
- Higher σ = wider spread of paths

---

### **Section 9: Final Summary**

**What it prints:**
- All market data (S0, K, T, r, q, σ)
- MC American call price ± confidence interval
- BSM European call price
- Difference between them
- IV statistics (min, max, ATM, mean)

**Key comparison:**
- MC American ≥ BSM European (early exercise premium)
- If q = 0, they should be very close (no early exercise benefit for calls)

---

## 🤔 Why Prices Might Be Smaller Than Expected

### **1. Dividend Yield (q) Reduces Value**

**The biggest factor!** When a stock pays dividends:
- Stock price drops by dividend amount on ex-dividend date
- Call option holders don't receive dividends
- This reduces option value significantly

**Example:**
- AAPL dividend yield ~0.5% annually
- Over 1 year, this reduces call value by ~$0.50-1.00
- For a $180 stock, this is meaningful!

### **2. At-the-Money (ATM) Strike**

We use `K = round(S0)`, which is ATM:
- **Intrinsic value = 0** (S0 - K = 0)
- All value is **time value**
- ATM options have lower prices than ITM (in-the-money) options

**If you want higher prices:**
- Try ITM: `K = S0 * 0.9` (10% below current price)
- This adds intrinsic value

### **3. Early Exercise Can Reduce Value**

For American calls with dividends:
- Early exercise might be optimal (to capture dividends)
- But this means giving up time value
- The LSMC algorithm finds the optimal exercise strategy
- This can result in lower prices than naive "never exercise early"

### **4. Historical Volatility Might Be Low**

- We use past year's volatility
- If market was calm, σ is low → lower option prices
- Real options use **implied volatility** (often higher)

### **5. Time to Expiration**

- T = 1.0 years (long time)
- But with dividends, time value is reduced
- Shorter expiration (e.g., 0.25 years) might show different behavior

### **6. Risk-Free Rate**

- r = 5% (hardcoded)
- Higher r → higher call prices (but also affects dividends)

---

## 💡 How to Get Higher Prices

If you want to see larger option prices, try:

1. **Lower strike (ITM):**
   ```python
   K = S0 * 0.85  # 15% below current price
   ```

2. **Higher volatility:**
   ```python
   sigma = 0.40  # 40% instead of ~28%
   ```

3. **No dividends:**
   ```python
   q = 0.0  # Remove dividend effect
   ```

4. **Longer expiration:**
   ```python
   T = 2.0  # 2 years instead of 1
   ```

---

## 🚀 Running the Notebook

1. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

2. **Launch Jupyter:**
   ```bash
   jupyter notebook
   ```

3. **Open notebook** and run cells sequentially

4. **Be patient** - convergence analysis takes time!

---

## 📈 Expected Output Ranges

For AAPL with typical parameters:
- **S0**: ~$180
- **K**: ~$180 (ATM)
- **BSM European**: ~$15-25
- **MC American**: ~$15-26 (slightly higher due to early exercise)
- **IV**: ~0.25-0.35 (25-35%)

**If prices are much lower:**
- Check dividend yield (q) - might be higher than expected
- Check volatility (σ) - might be lower
- Verify strike is ATM (not OTM)

---

## 🎓 Key Takeaways

1. **Dividends significantly reduce call option values**
2. **American options are more complex** (early exercise decisions)
3. **Monte Carlo converges** with more paths (but takes time)
4. **Real market data** shows volatility smile (not flat)
5. **ATM options** have lower prices than ITM options

---

## 🔧 Troubleshooting

**"Prices seem too low":**
- Check q (dividend yield) - this is the main culprit
- Try ITM strike (K < S0)
- Increase volatility

**"Convergence is slow":**
- This is normal! 100k paths takes time
- You can reduce max paths: `N_paths_list = [10, 50, 100, 500, 1000, 5000]`

**"Can't fetch option chain":**
- Network issue or yfinance API limit
- Notebook will use hypothetical data (still works!)

**"Import errors":**
- Make sure all packages installed: `pip install -r requirements.txt`

---

Enjoy exploring quantitative finance! 🚀

