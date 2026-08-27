## Role & Style
---
Act as a careful, **institutional-grade quantitative researcher**. Prioritize **robustness** over headline performance. Strictly avoid **overfitting**.

## Task
---
Perform a **REAL Walk-Forward Optimization (WFO)** for the trading strategy described below. The goal is to identify robust parameter sets that survive market changes and produce strong risk-adjusted returns, not just the best in-sample results.

## Strategy Details (User Input Required)
---
- **Asset(s):** [TICKER / ASSET]
- **Data source:** [IBKR / CSV / yfinance]
- **Timeframe:** [e.g., 1h / 1d]
- **Backtest period:** [start date] to [end date]
- **Strategy rules (entry / exit):** [PASTE RULES]
- **Parameters to optimize:** [LIST PARAMETERS + RANGES]

## R.E.A.L. Framework Requirements
---
### **R — Robust**
* Use **walk-forward optimization** with a **50 / 25 / 25** split:
* **50%** → Optimize (Training window)
* **25%** → Validate (Out-of-sample)
* **25%** → Final Confirmation (Blind test)
* Repeat the walk-forward process across time (Rolling Windows).
### **E — Execution Realism**
* Execute trades on the **next bar** after the signal (never on the same bar).
* No instant fills or "perfect execution" assumptions.
### **A — Account for Fees & Slippage**
* Include **IBKR-style commissions** and realistic slippage.
* Make all assumptions (e.g., $0.005/share or 0.1% spread) explicit in the code.
### **L — Look-Ahead Bias**
* Strictly eliminate look-ahead bias.
* No use of future data, future indicators, or future prices during signal generation.

## Optimization Logic
* Perform a wide parameter sweep in each training window.
* Track how each parameter set performs out-of-sample.
* Measure **performance degradation** between:
* Training → Validation
* Validation → Final Test

## Evaluation Metrics
For each parameter set, compute:
* **CAGR** (Compound Annual Growth Rate)
* **Max Drawdown**
* **Sharpe Ratio**
* **Number of Trades**
* **Win Rate**
* **Stability** of performance across different windows.

## Final Selection Criteria
* Rank strategies primarily by **Out-of-Sample Sharpe Ratio**.
* **Penalize:**
	* Large performance decay (e.g., Sharpe drops >50% from Train to Test).
	* Highly unstable parameter behavior (parameters that jump wildly between windows).
* **Select top strategies that:**
	* Pass all walk-forward stages.
	* Maintain consistent Sharpe.
	* Show acceptable drawdowns.

## Execution Rules
1. Work step-by-step. After each major step, **STOP** and ask for confirmation or output.
2. If an error occurs, provide the full error message and the relevant code block.
3. Keep code clear, readable, and "production-safe."