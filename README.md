# fastquant :nerd_face:
[![Build Status](https://travis-ci.com/enzoampil/fastquant.svg?branch=master)](https://travis-ci.com/enzoampil/fastquant)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/ambv/black)
[![License: GPL v3](https://img.shields.io/badge/license-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

## Bringing backtesting to the mainstream

**fastquant** allows you to easily backtest investment strategies with as few as 3 lines of python code. Its goal is to promote data driven investments by making quantitative analysis in finance accessible to everyone.

## Features
1. Easily access historical stock data
2. Backtest and optimize trading strategies with only 3 lines of code

<sup>`*` - Both Yahoo Finance and Philippine stock data data are accessible straight from fastquant<sup>

## Installation
```
pip install fastquant
```

## Get stock data
All symbols from [Yahoo Finance](https://finance.yahoo.com/) and Philippine Stock Exchange ([PSE](https://www.pesobility.com/stock)) are accessible via `get_stock_data`.

```
from fastquant import get_stock_data
df = get_stock_data("JFC", "2018-01-01", "2019-01-01")
print(df.head())

#           dt  close
#   2019-01-01  293.0
#   2019-01-02  292.0
#   2019-01-03  309.0
#   2019-01-06  323.0
#   2019-01-07  321.0
```

*Note: Symbols from Yahoo Finance will return closing prices in USD, while symbols from PSE will return closing prices in PHP*

## Backtest trading strategies

### Simple Moving Average Crossover (15 day MA vs 40 day MA)
Daily Jollibee prices from 2018-01-01 to 2019-01-01
```
from fastquant import backtest
backtest('smac', df, fast_period=15, slow_period=40)

# Starting Portfolio Value: 100000.00
# Final Portfolio Value: 102272.90
```
![](./docs/assets/smac_sample.png)

## Optimize trading strategies with automated grid search

### Simple Moving Average Crossover (15 to 30 day MA vs 40 to 55 day MA)
Daily Jollibee prices from 2018-01-01 to 2019-01-01

```
from fastquant import backtest
res = backtest("smac", df, fast_period=range(15, 30, 3), slow_period=range(40, 55, 3), verbose=False)

# Optimal parameters: {'init_cash': 100000, 'buy_prop': 1, 'sell_prop': 1, 'execution_type': 'close', 'fast_period': 15, 'slow_period': 40}
# Optimal metrics: {'rtot': 0.022, 'ravg': 9.25e-05, 'rnorm': 0.024, 'rnorm100': 2.36, 'sharperatio': None, 'pnl': 2272.9, 'final_value': 102272.90}

print(res[['fast_period', 'slow_period', 'final_value']].head())

#	fast_period	slow_period	final_value
#0	15	        40	        102272.90
#1	21	        40	         98847.00
#2	21	        52	         98796.09
#3	24	        46	         98008.79
#4	15	        46	         97452.92

```


## Library of trading strategies

| Strategy | Alias | Parameters |
| --- | --- | --- |
| Relative Strength Index (RSI) | rsi | `rsi_period`, `rsi_upper`,  `rsi_lower` |
| Simple moving average crossover (SMAC) | smac | `fast_period`, `slow_period` |
| Exponential moving average crossover (EMAC) | emac | `fast_period`, `slow_period` |
| Moving Average Convergence Divergence (MACD) | macd | `fast_perod`, `slow_upper`, `signal_period`, `sma_period`, `sma_dir_period` |
| Bollinger Bands | bbands | `period`, `devfactor` |
| Buy and Hold | buynhold | `N/A` |

### Relative Strength Index (RSI) Strategy
```
backtest('rsi', df, rsi_period=14, rsi_upper=70, rsi_lower=30)

# Starting Portfolio Value: 100000.00
# Final Portfolio Value: 132967.87
```
![](./docs/assets/rsi.png)

### Simple moving average crossover (SMAC) Strategy
```
backtest('smac', df, fast_period=10, slow_period=30)

# Starting Portfolio Value: 100000.00
# Final Portfolio Value: 95902.74
```
![](./docs/assets/smac.png)

### Exponential moving average crossover (EMAC) Strategy
```
backtest('emac', df, fast_period=10, slow_period=30)

# Starting Portfolio Value: 100000.00
# Final Portfolio Value: 90976.00
```
![](./docs/assets/emac.png)

### Moving Average Convergence Divergence (MACD) Strategy
```
backtest('macd', df, fast_period=12, slow_period=26, signal_period=9, sma_period=30, dir_period=10)

# Starting Portfolio Value: 100000.00
# Final Portfolio Value: 96229.58
```
![](./docs/assets/macd.png)

### Bollinger Bands Strategy
```
backtest('bbands', df, period=20, devfactor=2.0)

# Starting Portfolio Value: 100000.00
# Final Portfolio Value: 97060.30
```
![](./docs/assets/bbands.png)


See more examples [here](https://nbviewer.jupyter.org/github/enzoampil/fastquant/tree/master/examples/).
