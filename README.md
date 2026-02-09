# SPY Mean Reversion with 6-Month Trend Filter

This project backtests a quantitative trading strategy on the SPY ETF using daily data. The strategy combines short-term mean reversion with a long-term trend filter in order to control risk and avoid trading during unfavorable market regimes.

Data:
Asset: SPY ETF
Frequency: Daily
Data source: yfinance
Price used: Close
Start date: 2010-01-01

Daily returns are calculated using percentage change of closing prices.

Strategy Logic:
A 6-month momentum filter is used to define the long-term market regime. Six months are approximated as 126 trading days. Trades are allowed only when the 6-month momentum is positive.

mom_6m = Close.pct_change(126)
Condition: mom_6m > 0

A mean reversion entry signal is generated after a strong negative daily return. A buy signal occurs when the previous day return is less than or equal to −0.5% and the 6-month momentum is positive.

signal = 1 if:
(mom_6m > 0) AND (daily_return <= -0.005)

To avoid look-ahead bias, positions are entered one day after the signal using a shift operation.

position = signal.shift(1)

A rolling holding window is applied to the position:

position = signal.shift(1).rolling(5).max()

This keeps the strategy invested for up to five trading days after a signal. If new signals occur during this period, the holding window is extended.

Strategy returns are calculated as:

strategy_return = position * daily_return

The equity curve is computed using compounded returns:

equity = (1 + strategy_return).cumprod()

Evaluation:
The strategy is evaluated using the following metrics:
Final equity
Maximum drawdown
Sharpe ratio (annualized)
Time in market

Performance is compared against a Buy & Hold benchmark on SPY.

Validation:
To reduce overfitting risk, the strategy can be evaluated using a simple train/test split.
Train period: before 2017
Test period: 2017 onward

Results Summary:

The table below summarizes final equity and maximum drawdown for the strategy and a Buy & Hold benchmark.

Train period:
Strategy: Final equity 1.64, Max drawdown −15.7%
Buy & Hold: Final equity 2.28, Max drawdown −18.6%

Test period:
Strategy: Final equity 3.26, Max drawdown −25.6%
Buy & Hold: Final equity 8.17, Max drawdown −33.7%

The strategy underperforms Buy & Hold in terms of absolute returns, but consistently exhibits lower drawdowns in both train and test periods. This behavior is consistent with the strategy design, which prioritizes risk control and drawdown reduction over maximum return.
