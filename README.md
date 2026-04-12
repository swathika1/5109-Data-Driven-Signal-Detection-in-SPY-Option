---

# Options Mispricing Strategy using Machine Learning

## Overview

This project explores whether **machine learning can identify and exploit mispricing in SPY options**.
Instead of relying on traditional models like Black-Scholes, we train an **XGBoost model** to learn the empirical pricing structure of options and generate trading signals based on deviations between predicted and observed prices.

The key objective is to evaluate whether these deviations represent **true arbitrage opportunities** or are simply **artifacts of market microstructure (e.g., bid-ask spreads, liquidity constraints)**.

---

## Dataset

* Source: S&P 500 (SPY) Options Implied Volatility Dataset (Kaggle: https://www.kaggle.com/datasets/shankerabhigyan/s-and-p500-options-spy-implied-volatility-2019-24?select=spy_options_data_24.json)
* Period: Jan 2024 – Dec 2024
* Size: ~2.29M raw records → ~874K after cleaning
* Features include:

  * Contract details (strike, expiry, type)
  * Pricing (bid, ask, mid-price)
  * Greeks (delta, gamma, theta, vega)
  * Volatility and liquidity metrics
    
---

## Methodology

### 1. Data Preprocessing

* Removed invalid and illiquid contracts
* Applied filters on:

  * Implied volatility (1%–100%)
  * Relative spread (≤ 50%)
* Merged SPY spot price using **backward time alignment** to avoid look-ahead bias

---

### 2. Modelling

* **Target**: Option mid-price

* **Models Used**:

  * Linear Regression (baseline)
  * XGBoost (advanced model)

* **Data Split (Time-based)**:

  * Train: Jan–Sep
  * Validation: Sep–Nov
  * Test: Nov–Dec

* **Results**:

  * Linear Regression: R² ≈ 0.57
  * XGBoost: R² ≈ 0.99, MAE ≈ $1.78

---

### 3. Signal Construction

* **Raw Signal**:

  ```
  Signal = Market Price − Model Prediction
  ```

* **Economic Signal** (after costs):

  ```
  |Signal| − (0.5 × spread + hedge cost)
  ```

* Trades are triggered only when:

  * Signal exceeds **95th percentile threshold (validation set)**
  * Contracts are liquid (low spread)
  * Focus on near-ATM regions

---

### 4. Strategy Design

* **Delta-neutral strategy** to isolate pricing inefficiencies
* Each trade is held for one period (next quote)
* Two execution assumptions:

  1. **Midpoint pricing (idealized)**
  2. **Executable pricing (realistic: bid/ask crossing)**

---

## Key Results

| Metric           | Midpoint | Executable            |
| ---------------- | -------- | --------------------- |
| Sharpe Ratio     | ~2.17    | ~0.60                 |
| Win Rate         | 56%      | ~49%                  |
| Median Trade P&L | +78      | −14                   |
| Total P&L        | High     | Significantly reduced |

### Key Insight

Most of the apparent “alpha” disappears when realistic execution is applied.
-> The majority of detected mispricing lies **within the bid-ask spread**, making it **non-exploitable in practice**.

---

## Additional Analysis

* **Volatility Regimes**:

  * Profitable mainly in **high-volatility periods**
  * Loss-making in low-vol regimes

* **Moneyness**:

  * Best performance: Near-ATM options
  * Poor performance: Illiquid tail options

* **Greeks**:

  * Moderate gamma & vega exposure performs best
  * High gamma trades are unstable due to hedging risk

---

## Limitations

* Model may be learning **market pricing function**, not true mispricing
* Execution assumptions exclude:

  * Slippage
  * Market impact
  * Partial fills
* Static delta hedging (no dynamic rebalancing)
* Single-year dataset limits robustness

## Conclusion

While machine learning can accurately model option prices,
**real-world trading frictions eliminate most arbitrage opportunities**.

The strategy behaves as a **conditional alpha strategy**,
working only under specific conditions (e.g., high volatility),
rather than a consistent arbitrage mechanism.

---


 
