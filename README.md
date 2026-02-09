# Mean-Reversion-Strategy-with-Trend-Filter
Overview

This project implements a simple mean reversion trading strategy on the SPY ETF, enhanced with a long-term trend filter.
The idea is to buy short-term market dips only when the broader market trend remains positive (wich is a 6 month trend).

The strategy is designed to reduce large drawdowns while still capturing a significant portion of market upside.

Strategy Logic

The strategy follows two core principles:

Mean Reversion (Entry Condition)

Enter the market if the previous trading day had a strong negative return (e.g. ≤ −0.5%).

This represents short-term panic or overreaction.

Trend Filter (Risk Control)

Trades are only allowed if the 6-month momentum is positive.

This avoids buying dips during long-term downtrends.

Positions are entered the day after the signal to avoid look-ahead bias.

Data

Asset: SPY ETF

Frequency: Daily

Data source: yfinance

Price used: Close

Returns: daily percentage returns

Implementation Details

Signal

signal = 1 if:

previous day return ≤ −0.5%

AND 6-month return > 0

Position

position = signal.shift(1)

Strategy Return

strategy_return = position × daily_return

Equity Curve

Computed using cumulative compounded returns

Performance Metrics

The strategy is evaluated using:

Final Equity

Maximum Drawdown

Sharpe Ratio (annualized)

Time in Market

Results are compared against a Buy & Hold benchmark on SPY.

Train / Test Validation

To reduce overfitting risk, the strategy is evaluated using a train/test split:

Train period: before 2017

Test period: 2017 onward

The strategy maintains similar behavior across both periods, suggesting that results are not driven solely by curve fitting.

Risk Considerations

This strategy is not risk-free. Potential risks include:

Large market crashes where mean reversion fails

Underperformance during strong bull markets

Fewer trading opportunities due to strict filters

The strategy prioritizes drawdown control over maximum possible return.

Conclusion

This project demonstrates a complete quantitative research pipeline:

hypothesis-driven strategy

clean signal construction

proper backtesting

risk-aware evaluation

out-of-sample validation

The goal is not to maximize historical returns, but to build a robust and understandable trading framework.
Add project README
