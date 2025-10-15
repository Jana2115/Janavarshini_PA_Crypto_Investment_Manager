# Milestone – 5: Momentum-Based Portfolio Stress Test

## Task 1: 

## Dataset Loading & Preparation
### Code Summary
- Load `crypto-markets.csv`.
- Filter only **BTC** and **ETH**.
- Parse dates and sort by symbol and date.

### Output
- Clean DataFrame `df` ready for return calculations.

---

## Daily Returns Calculation

### Code Summary
- Compute **daily % returns** for each coin.
- Align lengths for portfolio computation.

### Output
- DataFrame `returns_df` → daily returns of BTC & ETH.

---

## Momentum Rule – Dynamic Portfolio Weights

### Code Summary
- Compute mean returns over a rolling window (default 5 days).
- Assign **higher weights to assets with positive momentum**.
- Normalize weights to sum to 1.
- Use **equal allocation** if momentum ≤ 0.

### Output
- List of dictionaries `daily_weights` containing BTC & ETH weights per day.

---

## Portfolio Returns with Dynamic Weights

### Code Summary
- Compute daily portfolio return = weighted sum of BTC & ETH returns.
- Uses daily weights from momentum rule.

### Output
- Series `portfolio_ret` → portfolio daily returns.

---

## Stress Test Scenarios

### Code Summary
- Define **3 scenarios**:
  1. **Bull Market** → all positive returns.
  2. **Bear Market** → all negative returns.
  3. **Volatile Market** → alternating positive & negative returns.

- Scenarios generated dynamically for the number of days in `daily_weights`.

### Output
- Dictionary of DataFrames, each with returns per coin for all scenarios.

---

## Apply Stress Test to Portfolio

### Code Summary
- Multiply scenario returns with daily momentum weights.
- Compute **portfolio returns under each stress scenario**.

### Output
- DataFrame `stress_df` indexed by Day → columns: Bull Market, Bear Market, Volatile Market.

---

## Store Results in SQLite Database

### Code Summary
- Create table `stress_results_momentum`.
- Insert daily weights + stress test portfolio returns.
- Each row contains:
  - Date, BTC weight, ETH weight
  - Bull, Bear, Volatile market portfolio returns

### Output
- SQLite DB `crypto_stress_test_momentum.db` with complete stress test results.

---

## Main Execution Flow

1. Load & prepare dataset.
2. Compute daily returns.
3. Calculate **momentum-based dynamic weights**.
4. Compute portfolio daily returns.
5. Apply stress test scenarios.
6. Print results.
7. Save results to SQLite database.

---

## Task 2:

## Stress Test Portfolio Visualization

### Code Summary
- Select the first 15 days of `stress_df` for plotting.
- Create a **line plot** for each scenario:
  - Bull Market
  - Bear Market
  - Volatile Market
- Use markers for each point to improve readability.
- Configure plot aesthetics:
  - Rotate x-axis labels (Days) for clarity.
  - Add axis labels, title, legend, and grid.
  - Adjust layout for better fit.

---

### Output
- Line chart showing **portfolio returns under different stress scenarios** for the first 15 days.
- Each scenario is represented with a separate line and marker.

---

### Sample Visualization Description
- **X-axis:** Days (Day 1 to Day 15)
- **Y-axis:** Portfolio Return
- **Lines:**
  - **Bull Market:** Positive trend
  - **Bear Market:** Negative trend
  - **Volatile Market:** Alternating positive and negative returns
- Highlights **how portfolio reacts under different market conditions**.

---

### Insights
- Portfolio returns are **stable in Bull and Bear markets**, following expected trends.
- Volatile Market shows **high fluctuations**, emphasizing the need for risk management.
- Visualization helps **quickly identify scenario-specific portfolio behavior**.
