# Goal

Create universal functions

1. Get history data

   - series

   - dataFrame

   - Exchange:

     - dYdX → Dex

     - Binance → Biggest

     - Bybit → ready production

     -BitMex → Rebate

     - IG → Forex

2. Find Cointegration

   - indicators

   - vs. Crypto Wizards

   - One-click Backtesting

3. Backtesting

   - python class

   - Plots

   - vs. Crypto Wizards

   - One-click Placing order

4. Monitor signals

5. Entry / exit

   - Stop Loss

   - P&L report for each pair

     - Entry / Exit amount & Z-score

6. Cloud

7. Telegram

# Statistical Arbitrage

Pair Trading → Neutral

RISK: One loss can be big although high win rate!

## Cointegration

- Cointegration != Correlation

- Cointegration means 2 pairs `cross` each other often.

```python
from statsmodels.tsa.stattools import coint

coint_res = coint(series_1, series_2)
```

## Hedge Ratio

```python
hedge_ratio = model.params[0]
```

Note: for forex, it's more complicated, needs to consider `pip value`

## Spread

The distance of the 2 prices of the pair

```python
spread = series_1 - (hedge_ratio * series_2)
```

## Z-Score

Signals to trade

```python
zscore = (x - mean) / std

# Z-score is calculated using the spread, which is used as the series.
```

## Half-Life

The half-life in this context is a statistical measure that indicates the period of time it takes for a quantity to reduce to half its initial value.

aka. How ling an average will it take for our spread to revert back.

```python
def calculate_half_life(spread):
   df_spread = pd.DataFrame(spread, columns=["spread"])
   spread_lag = df_spread.spread.shift(1)
   # To avoid a NaN value, the code replaces the first element with the value of the second element.
   spread_lag.iloc[0] = spread_lag.iloc[1]
   spread_ret = df_spread.spread - spread_lag
   spread_ret.iloc[0] = spread_ret.iloc[1]
   spread_lag2 = sm.add_constant(spread_lag)
   model = sm.OLS(spread_ret, spread_lag2)
   res = model.fit()
   halflife = round(-np.log(2) / res.params[1], 0)
   return halflife
```

> Recommendation: 0 < half-life < 24

## Kelly Criterion

How much buying power do we have? Especially as we get drawdown

1. If you have no edge, don't trade!

2. Do not use leverage!

3. We need at least 54% when taking into account trading cost.

4. Start with only `1%` of the capital

# Connect to dYdX V3

1. Get Alchemy (HTTP provider)

2. Connect wallet → enable `Remember Me `

3. Chrome browser → Inspection → Application → Storage → Local storage → https://trade.stage.dydx.exchange (Testnet) or https://trade.dydx.exchange (Mainnet)

   - STARK_KEY_PAIRS → wallet address

     - privateKey

   - API_KEY_PAIRS → wallet address

     - key

     - secret

     - passphrase

# Git

1.
