#  Milestone – 3: Cryptocurrency Portfolio Risk Monitoring & Linear Regression Prediction 

## Task 1: (Risk_checker.py)

## Data Preparation
- Loads dataset (`crypto-markets.csv`).  
- Filters only **BTC, ETH, USDT, DOGE**.  
- Converts date to datetime format and sorts by `symbol` and `date`.  
- Keeps only the required column → **close price**.  

### Output  
- Cleaned **DataFrame** ready for risk analysis.  

## Risk Metrics Calculation  

### Metrics Implemented  
1. **Annualized Volatility** → Risk from daily fluctuations.  
2. **Annualized Return** → Expected yearly growth.  
3. **Sharpe Ratio** → Risk-adjusted return.  
4. **Maximum Drawdown** → Largest portfolio fall from a peak.  
5. **Sortino Ratio** → Penalizes only downside volatility.  
6. **Beta vs Market (BTC)** → Measures sensitivity to BTC (proxy for crypto market).  
7. **Max Asset Weight** → Ensures diversification.  

### Output  
- Each metric computed with a **pass/fail status** depending on thresholds:  

| Metric              | Threshold   | Pass Criteria              |
|---------------------|-------------|----------------------------|
| Volatility (annual) | ≤ 5%        | Lower = safer              |
| Sharpe Ratio        | ≥ 1.0       | Higher = better            |
| Max Drawdown        | ≥ -20%      | Avoid deep losses          |
| Sortino Ratio       | ≥ 1.0       | Protect downside           |
| Beta vs BTC         | ≤ 1.2       | Lower = less risky         |
| Max Asset Weight    | ≤ 40%       | Prevent overexposure       |

## Database Storage  

### Code Summary  
- Creates SQLite database **risk_checks.db**.  
- Stores results in two tables:  
  - **risk_results** → All metrics with values and pass/fail flags.  
  - **rule_failures** → Only failed rules with threshold info.  

### Output  
- Database keeps track of **historical risk checks** with timestamps.  

---

## Email Alert System  

### Code Summary  
- If any metric **fails threshold check**, details are compiled into an alert.  
- Uses **SMTP (Gmail)** to send an email notification.  
- Email includes:  
  - Failed rules with metric values.  
  - Full metrics snapshot for reference.  

### Output  
- Sends mail with subject:  
  **“ALERT: Risk rule(s) failed for portfolio”**  

---

## Execution Flow  

### Steps  
1. Load dataset.  
2. Compute daily returns.  
3. Evaluate all risk rules.  
4. Store results in SQLite.  
5. If failures detected → Trigger **email alert**.  
6. Print risk summary and sample portfolio returns.  

---

## Sample Console Output  

```
Risk Check Summary:

Volatility_annual: value = 0.032, pass = True
Sharpe: value = 0.85, pass = False
Max_Drawdown: value = -0.18, pass = True
Sortino: value = 0.90, pass = False
Beta: value = 1.10, pass = True
Max_Asset_Weight: value = 0.40, pass = True

RULE FAILURES !!!
- Sharpe: 0.85
- Sortino: 0.90

Alert mail sent.

Sample portfolio returns (first 10 non-null days):
  1. 2021-01-05 -> 0.0035
  2. 2021-01-06 -> -0.0012
  ...
```

---

## Insights  

- **Volatility**: Portfolio shows controlled fluctuations.  
- **Sharpe & Sortino Ratios**: Below threshold → portfolio not offering enough risk-adjusted return.  
- **Max Drawdown**: Acceptable (within -20%).  
- **Beta**: Acceptable, portfolio not overly sensitive to BTC market moves.  
- **Diversification**: Maintained (no asset > 40%).  

---

**Conclusion**:  
The system automates **risk monitoring** with rule checks, storage, and email alerts.  
It provides early warnings when the portfolio fails to meet safety thresholds, making it useful for **real-time crypto portfolio risk management**.  

# Task 2: (Predictor.py)

## Objective
This milestone enhances the crypto portfolio analysis by applying **Linear Regression** to model daily returns, evaluate model performance using **R²** and **MAE**, and predict future returns for both individual coins and the overall portfolio.

---

## Key Steps

### 1. Dataset Loading & Preparation
- Load crypto market dataset (`crypto-markets.csv`).
- Filter for selected coins: **BTC, ETH, USDT, DOGE**.
- Ensure proper formatting of `symbol`, `date`, and `close` columns.

### 2. Compute Daily Returns
- Calculate percentage change (`pct_change`) for each coin’s daily closing prices.
- Store results in a combined `returns_df` DataFrame.

### 3. Portfolio Returns
- Compute portfolio returns as a weighted sum of individual coin returns:
  - **BTC** → 50%
  - **ETH** → 25%
  - **USDT** → 15%
  - **DOGE** → 10%

### 4. Linear Regression Model
- Features (`X`): Time index (days).
- Target (`y`): Daily returns of coin or portfolio.
- Fit linear regression model using **sklearn**.
- Generate predictions:
  - Historical fit (`y_pred`).
  - Future 5-day predictions (`preds`).

### 5. Performance Metrics
- **R² (Coefficient of Determination):** Goodness of fit.
- **MAE (Mean Absolute Error):** Average prediction error.

### 6. Visualization
- Plot includes:
  - Actual returns (blue).
  - Regression fit (red dashed line).
  - Future predictions (green dots).
- Title includes metrics (**R²** and **MAE**).

---

## Example Output (Console)

```text
==== Predictions and Metrics ====

BTC next 5-day predicted returns: [0.0023 0.0025 0.0026 0.0028 0.0030]
BTC R²=0.62, MAE=0.015

ETH next 5-day predicted returns: [0.0011 0.0012 0.0013 0.0015 0.0016]
ETH R²=0.54, MAE=0.018

USDT next 5-day predicted returns: [0.0000 0.0000 0.0000 0.0000 0.0000]
USDT R²=0.01, MAE=0.001

DOGE next 5-day predicted returns: [0.0045 0.0047 0.0049 0.0050 0.0052]
DOGE R²=0.45, MAE=0.020

Portfolio next 5-day predicted returns: [0.0018 0.0020 0.0021 0.0022 0.0023]
Portfolio R²=0.58, MAE=0.012
```

---

## Insights
- **BTC & ETH** show moderate predictability with decent R² values.
- **USDT** remains nearly flat, as expected for stablecoins.
- **DOGE** shows higher volatility but still follows a linear trend.
- The **Portfolio** smooths out extreme variations, offering more stable predictions.

---

## Deliverables
- Linear regression modeling of returns for **individual coins** and **portfolio**.
- Performance metrics (**R², MAE**) for evaluation.
- Plots visualizing **historical returns, regression fit, and 5-day predictions**.
- Enhanced insight into **portfolio risk and growth trends**.