# Project 3: Stock Trading with RL

Apply reinforcement learning to algorithmic trading - a different domain from games and robotics!

---

## 💰 Overview

**Stock trading** is a natural fit for RL because:
- Sequential decision-making (when to be in or out of the market)
- Delayed rewards (profit realized over time)
- Uncertain environment (market dynamics)
- Clear success metric (profit/loss)

**Challenge:** Markets are non-stationary, noisy, and complex!

**Goal:** Build a trading agent that outperforms buy-and-hold strategy

---

## 🚀 Quick Start

The baseline uses **gym-anytrading** with real historical stock data.

### Requirements
```bash
pip install stable-baselines3 gym-anytrading pandas matplotlib
```

### Run Baseline (a few minutes)
Open `project3_stock_trading.ipynb` and run all cells. This will:
1. Load historical AAPL stock data
2. Train an A2C agent to trade (buy/sell)
3. Backtest on test data and visualize results

**Expected result:** do not expect the baseline to beat buy-and-hold. With only the
closing price as input, the A2C baseline often settles on one action every day -
always out of the market (0% return) or always in it (buy-and-hold minus fees). The
notebook prints how often it chose each action so you can see which happened.

That is the starting point, not a bug: getting the agent to actually trade, and to
trade well, is the project.

---

## 📊 Baseline Performance

| Strategy | Test return |
|----------|-------------|
| Buy-and-hold | whatever the stock did over the test window (AAPL rose over 2023) |
| Never invested | 0% |
| A2C baseline | usually one of the two above, depending on which constant action it settles on |

**Note:** performance depends heavily on the market period - a rising market makes
buy-and-hold hard to beat.

---

## 💡 Ideas to Improve

### 1. **Feature Engineering**
Add technical indicators:
```python
# Current: closing price and its daily change only
# (stocks-v0 ignores Open/High/Low/Volume - override _process_data to use them)
# Add:
- Moving averages (MA, EMA)
- RSI (Relative Strength Index)
- MACD (Moving Average Convergence Divergence)
- Bollinger Bands
- Volume indicators
- Sentiment scores (news, social media)
```

### 2. **Reward Shaping**
```python
# Simple: just profit
reward = position * price_change

# Better: risk-adjusted
reward = profit - risk_penalty

# Advanced: Sharpe ratio
reward = (return - risk_free_rate) / volatility

# Multi-objective
reward = alpha * profit + beta * (-risk) + gamma * turnover_cost
```

### 3. **Transaction Costs**
Make it realistic:
- Bid-ask spread
- Commission fees (~$1-10 per trade)
- Slippage (price moves during execution)
- Market impact (large orders move price)

### 4. **Position Sizing**
Instead of binary (all-in or all-out):
- Continuous actions: % of portfolio to allocate
- Kelly criterion for optimal sizing
- Risk management rules (max 20% per position)

### 5. **Multiple Assets**
Trade a portfolio:
- Select from S&P 500 stocks
- Include bonds/commodities for diversification
- Correlation-aware allocation
- Sector rotation strategies

---

## 🔬 Advanced Extensions

### 1. **Multi-Timeframe Analysis**
Use multiple data frequencies:
- Intraday: 1-min, 5-min bars
- Daily: OHLC data
- Weekly/Monthly: trends
- Learn hierarchical strategies

### 2. **Market Regime Detection**
Adapt to different market conditions:
- Bull market (trending up)
- Bear market (trending down)
- Sideways/ranging market
- High volatility periods
- Train separate policies per regime

### 3. **Ensemble Methods**
Combine multiple agents:
- Trend-following agent
- Mean-reversion agent
- Volatility arbitrage agent
- Meta-learner to weight strategies

### 4. **Options Trading**
More complex instruments:
- Call/put options
- Greeks (delta, gamma, theta)
- Implied volatility
- Multi-leg strategies (spreads, butterflies)

### 5. **Risk Management**
Professional risk controls:
- Stop-loss orders
- Position limits
- VaR (Value at Risk)
- Drawdown constraints
- Exposure limits per sector

### 6. **Offline RL**
Learn from historical data without live trading:
- Use CQL (Conservative Q-Learning)
- Prevents overestimation from off-policy data
- Safer for real deployment

---

## 📈 Evaluation Metrics

Financial metrics to track:
- **Total return** (%)
- **Sharpe ratio** (risk-adjusted return)
- **Maximum drawdown** (largest peak-to-trough loss)
- **Win rate** (% of profitable trades)
- **Profit factor** (gross profit / gross loss)
- **Sortino ratio** (downside risk-adjusted)
- **Calmar ratio** (return / max drawdown)

Compare against:
- Buy-and-hold
- Random strategy
- Simple moving average crossover
- Market index (S&P 500)

---

## 🎯 Project Deliverables

**Minimum** (complete baseline):
- Run baseline on AAPL
- Document returns and Sharpe ratio
- Compare to buy-and-hold

**Standard** (improvements):
- Add 3-5 technical indicators
- Test on multiple stocks
- Implement transaction costs
- Backtest on different time periods

**Advanced** (extensions):
- Multi-asset portfolio
- Regime-based strategies
- Risk management system
- Walk-forward optimization

---

## 📚 Resources

**Libraries:**
- [gym-anytrading](https://github.com/AminHP/gym-anytrading)
- [FinRL](https://github.com/AI4Finance-Foundation/FinRL) (more advanced)
- [TA-Lib](https://github.com/mrjbq7/ta-lib) (technical indicators)

**Data Sources:**
- Yahoo Finance (free, built-in)
- Alpha Vantage API
- Quandl
- Interactive Brokers API

**Papers:**
- [Deep RL for Trading](https://arxiv.org/abs/1907.04373)
- [Practical Deep RL for Stock Trading](https://arxiv.org/abs/2011.09607)

---

## 🐛 Troubleshooting

**Agent always picks the same action (doesn't trade):**
- Reward signal too weak, or the input features carry too little information
- Normalize the features: raw prices in the hundreds are hard for a small network
- Check the transaction costs are not so high that trading never pays
- Use shaped rewards (intermediate signals)

**Overfitting to training data:**
- Use longer training period (2+ years)
- Add noise/perturbations
- Cross-validation on multiple periods
- Use offline RL methods

**Unrealistic returns:**
- Add transaction costs
- Model slippage
- Use realistic position sizes
- Test on out-of-sample data

**High variance in results:**
- Average over multiple random seeds
- Test on multiple stocks
- Use different train/test periods

---

## ⚠️ Important Disclaimers

1. **Past performance ≠ future results**
2. **This is for educational purposes only**
3. **Not financial advice**
4. **Real trading involves risk of loss**
5. **Markets are non-stationary (models degrade over time)**
6. **Transaction costs matter significantly**
7. **Be extremely careful with real money**

**Recommendation:** Only use for learning. If trading real money, start with paper trading first!

---

## 💡 Tips

- **Start simple:** Single stock, daily data, basic features
- **Be realistic:** Include all costs and constraints
- **Out-of-sample testing:** Never test on training data!
- **Multiple periods:** Test across different market regimes
- **Risk first:** Prioritize not losing money over making money
- **Validation:** Compare to simple baselines (moving averages)

---

Good luck! 📈 Trading is challenging but great for learning about real-world RL applications.
