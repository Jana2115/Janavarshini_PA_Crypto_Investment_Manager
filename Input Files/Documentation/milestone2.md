#  Milestone – 2: Cryptocurrency Portfolio Analysis

This milestone focuses on analyzing cryptocurrency data (BTC, ETH, USDT, DOGE) using Python, Pandas, NumPy, SQLite, and Matplotlib.  
We calculate asset-level metrics, portfolio performance under different strategies, and visualize results.

---

##  Task 1: Asset Metrics & Portfolio Risk/Return

### Code Summary
- Load dataset and filter only **BTC, ETH, USDT, DOGE**.
- Define custom functions:
  - **Percent Change** – daily % price movement.
  - **Moving Average** – 7-day rolling average.
  - **Volatility** – risk measurement via standard deviation.
  - **Trading Signal** – BUY, SELL, HOLD.
  - **Portfolio Return** – weighted combination of assets.
- Compute metrics for each coin → stored in `assets_df`.
- Assign weights (BTC 50%, ETH 25%, USDT 15%, DOGE 10%).
- Compute **Portfolio Return** & **Portfolio Risk**.

### Output
- **Assets DataFrame** → Avg % Change, Volatility, Moving Avg, Signal.
- **Portfolio Summary** → Weighted Return & Risk.

---

## Task 2: Database Storage

### Code Summary
- Same process as Task 1 (metrics + portfolio).
- Stores results in **two databases**:
  - `portfolio_assets.db` → coin-level metrics table.
  - `portfolio.db` → portfolio-level summary table.

### Output
- **portfolio_assets** (detailed per coin):  
  - Currency | Avg_Percent_Change | Volatility | Moving_Avg | Signal
- **portfolio** (single row summary):  
  - Portfolio_Return | Portfolio_Risk

---

## Task 3: Multiple Allocation Rules with Parallel Execution

### Allocation Rules
1. **Equal Weight** → All coins same weight.
2. **Market Capitalization** → BTC 50%, ETH 25%, USDT 15%, DOGE 10%.
3. **Performance-Based** → Higher return = higher weight.

### Code Features
- Uses **ThreadPoolExecutor** for parallel execution.
- Computes portfolio return & risk under each rule.
- Saves into **SQLite DB (`portfolio_results.db`)**:
  - `portfolio_assets` → metrics for each coin.
  - `portfolio_rules` → results for each allocation strategy.

---

## Task 4: Portfolio vs Single Asset Returns (Visualization)
 
This project compares the performance and risk of individual cryptocurrencies (BTC, ETH, USDT, DOGE) against a weighted portfolio created using the **Market Capitalization rule**.  

---

##  Steps Performed  

### 1. Dataset Preparation  
- Loaded the `crypto-markets.csv` dataset.  
- Filtered only **BTC, ETH, USDT, DOGE**.  
- Sorted by date and selected **first 15 rows per coin** for sampling.  

---

### 2. Daily Returns Calculation  
- Used **percentage change formula** to compute daily returns for each coin.  
- Stored in a combined DataFrame `returns_df`.  

Formula:  
\[
\text{Daily Return} = \frac{P_t - P_{t-1}}{P_{t-1}} \times 100
\]

---

### 3. Portfolio Weights (Market Cap Rule)  
- **BTC → 50%**  
- **ETH → 25%**  
- **USDT → 15%**  
- **DOGE → 10%**  

Portfolio daily return = weighted sum of all assets.  

---

### 4. Visualizations  

####  Cumulative Returns  
- Plots daily cumulative returns of each coin and the portfolio.  
- Black line = Portfolio (more stable trend).  

####  Volatility (Risk) Comparison  
- Risk measured as **standard deviation of daily returns**.  
- Line plot shows volatility for BTC, ETH, USDT, DOGE vs Portfolio.  

---

### 5. Risk Table  

| Asset     | Volatility (%) |
|-----------|----------------|
| BTC       | ~Low (stable)  |
| ETH       | Medium risk    |
| USDT      | Very low (stablecoin) |
| DOGE      | Very high (volatile)  |
| Portfolio | Balanced (lower than risky assets) |

---

### 6. Insights  
- **BTC & USDT** → act as stabilizers with low volatility.  
- **ETH** → moderate risk and growth potential.  
- **DOGE** → highly volatile, risky but with sharp gains/losses.  
- **Portfolio** → smoothens extremes, offering **lower overall risk** than holding volatile assets alone.  

---

### 7. Exported Files  
- `portfolio_daily_returns.csv` → contains daily returns of BTC, ETH, USDT, DOGE, and the Portfolio.  

---

 **Portfolio diversification smoothens volatility and provides a safer growth path compared to investing in single volatile coins like DOGE.**
