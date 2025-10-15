#  Multi-Coin Cryptocurrency Analysis with SQLite & Parallel Processing

This project analyzes cryptocurrency market data (BTC, ETH, USDT, DOGE) by computing financial metrics, storing them in a SQLite database, and running parallel tasks for efficiency.

---

##  Features Implemented

### 1. Dataset Preparation
- Loads **crypto-markets.csv**.
- Filters only **BTC, ETH, USDT, DOGE**.
- Cleans coin names to avoid duplicates (uppercased, stripped).
- Sorts data by **symbol** and **date** for time-series analysis.

---

### 2. Utility Functions
- **clean_numeric(series)** → Converts values to numeric, removes unwanted characters.
- **percent_change(prices)** → Computes daily % change in closing prices.
- **moving_average(prices, window=3)** → Calculates rolling average.
- **calculate_volatility(prices)** → Risk measure as standard deviation of % changes.
- **trading_signal(prices)** → Generates BUY/SELL/HOLD signals:
  - BUY → if daily return > 2%
  - SELL → if daily return < -2%
  - HOLD → otherwise
- **portfolio_return(assetA, assetB, wA=0.6, wB=0.4)** → Computes portfolio returns using weighted averages of two assets.

---

### 3. Database Handling (SQLite)
- **store_in_db_multi()**:
  - Saves full dataset into `crypto_parallel.db`.
  - Table created → `cryptodata`.
  - Returns a **preview of 10–20 rows per coin**.
- Allows quick inspection of BTC, ETH, USDT, and DOGE records.

---

### 4. Parallel Execution of Metrics
Uses **ThreadPoolExecutor** to calculate multiple metrics simultaneously:
- BTC metrics:
  - Percentage Changes (first 10 values)
  - Simple Moving Average (first 10 values)
  - Volatility (single numeric value)
  - Trading Signals (first 10 recommendations)
- ETH metrics (if ETH exists):
  - Percentage Changes (first 10 values)
  - Simple Moving Average (first 10 values)
  - Volatility
  - Trading Signals
- Portfolio Returns combining BTC + ETH (first 15 values)

Parallel execution improves performance on larger datasets.

---

### 5. Output
- **Database Preview** → Prints rows per coin (BTC, ETH, USDT, DOGE).
- **Metrics Results** → Prints each metric in a clean format.