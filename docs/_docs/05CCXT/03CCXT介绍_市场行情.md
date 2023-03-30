---
title: CCXT介绍_市场行情
category: CCXT
order: 3
---

## API

CCXT提供的API分为三类: 公有API, 私有API和统一API. 

**公有API**

- 交易对
- 价格流
- 委托账本（L1、L2、L3...）
- 交易历史（完成的订单、交易、执行）
- 行情数据（现价、24小时价格）
- 用于图表的OHLCV序列数据
- 其他公开访问点

**私有API**

- 管理个人账户信息
- 查询账户余额
- 委托市价单和限价单
- 创建充值地址并进行账户充值
- 请求提取法币和加密货币
- 查询个人的敞口/完结委托单
- 查询杠杆交易的位置
- 获取账本历史
- 在不同账户之间转账
- 使用商户服务

**统一API**

这些API有小驼峰和下划线两种写法, 如"fetchOHLCV"也可以写成"fetch_ohlcv"

1. 所有交易所中的公共方法的集合
2. 大部分都可以接受params参数
3. 部分API需要分页显示结果
- fetchTrades
- fetchOHLCV
- fetchOrders
- fetchOpenOrders
- fetchClosedOrders
- fetchMyTrades
- fetchTransactions
- fetchDeposits
- fetchWithdrawals

**交易所异常**

- NotSupported：如果交易所的API不支持所访问的端结点，就会抛出这个异常
- AuthenticationError：如果API需要身份验证而请求中没有提供或者提供的不正确，就会抛出这个异常
- PermissionDenied：如果请求中指定的api key没有足够的权限，就会抛出这个异常
- InsufficientFunds：如果账户余额不足以执行当前请求的操作，例如委托下单，就会抛出这个异常
- InvalidAddress：如果请求中的地址格式不正确，就会抛出这个异常
- InvalidOrder：这是统一API中order系列方法异常类的基类
- OrderNotFound：试图查询或取消不存在的委托单时，就会抛出这个异常

```python
# 获取全部属性和方法

print(dir(ccxt.okx()))

# 获取全部API接口
print(exchange.has)
```

## 订单簿模型

```json
{
  'bids': [
      [ price, amount ], // [ float, float ]
      [ price, amount ],
      ...
  ],
  'asks': [
      [ price, amount ],
      [ price, amount ],
      ...
  ],
  'symbol': 'ETH/BTC', // a unified market symbol
  'timestamp': 1499280391811, // Unix Timestamp in milliseconds (seconds * 1000)
  'datetime': '2017-07-05T18:47:14.692Z', // ISO8601 datetime string with milliseconds
  'nonce': 1499280391811, // an increasing unique identifier of the orderbook snapshot
}
```

1. 获取订单簿: fetch_order_book
2. 如果交易所不提供时间戳, 返回值中也没有 

- async fetchOrderBook (symbol, limit = undefined, params = {})
- async fetchOrderBooks (symbols = undefined, limit = undefined, params = {})

```python
import ccxt
from ccxt_passward import BinanceKey

exchange = ccxt.binance(
    {
    'apiKey': BinanceKey.apikey.value,
    'secret': BinanceKey.secret.value,
    }
)

# 统一API添加params参数
params = {}
symbol = "BTC/USDT"
result = exchange.fetch_order_book(symbol, limit=3, params=params)
print(result)

# 计算当前买卖价差
orderbook = exchange.fetch_order_book(exchange.symbols[0])

bid = orderbook['bids'][0][0] if len (orderbook['bids']) > 0 else None
ask = orderbook['asks'][0][0] if len (orderbook['asks']) > 0 else None
spread = (ask - bid) if (bid and ask) else None
print (exchange.id, 'market price', { 'bid': bid, 'ask': ask, 'spread': spread })
```

## 市场行情

```json
{
    'symbol':        string symbol of the market ('BTC/USD', 'ETH/BTC', ...)
    'info':        { the original non-modified unparsed reply from exchange API },
    'timestamp':     int (64-bit Unix Timestamp in milliseconds since Epoch 1 Jan 1970)
    'datetime':      ISO8601 datetime string with milliseconds
    'high':          float, // highest price
    'low':           float, // lowest price
    'bid':           float, // current best bid (buy) price
    'bidVolume':     float, // current best bid (buy) amount (may be missing or undefined)
    'ask':           float, // current best ask (sell) price
    'askVolume':     float, // current best ask (sell) amount (may be missing or undefined)
    'vwap':          float, // volume weighed average price
    'open':          float, // opening price
    'close':         float, // price of last trade (closing price for current period)
    'last':          float, // same as `close`, duplicated for convenience
    'previousClose': float, // closing price for the previous period
    'change':        float, // absolute change, `last - open`
    'percentage':    float, // relative change, `(change/open) * 100`
    'average':       float, // average price, `(last + open) / 2`
    'baseVolume':    float, // volume of base currency traded for last 24 hours
    'quoteVolume':   float, // volume of quote currency traded for last 24 hours
}
```

注意：

- bidVolume指的是委托账本中当前的最优买入价委托单的总量
- askVolume指的是委托账本中当前的最优卖出价委托单的总量
- baseVolume指的是过去24小时内基准货币的交易量（买入或卖出）
- quoteVolume指的是过去24小时内报价货币的交易量（买入或卖出）

行情结构中的所有价格都是以报价货币计量，其中某些字段可能是undefined / None / null。

## OHLCV数据结构

fetchOHLCV(symbol, timeframe = '1m', since = undefined, limit = undefined, params = {})
- since参数是一个以毫秒计量的UTC时间戳

```json
[
    [
        1504541580000, // UTC 时间戳，单位：毫秒
        4235.4,        // (O)开盘价格, float
        4240.6,        // (H)最高价格, float
        4230.0,        // (L)最低价格, float
        4230.7,        // (C)收盘价格, float
        37.72941911    // (V)交易量，以基准货币计量， float
    ],
    ...
]
```

结果数组是以时间升序排列的，最早的烛线排在第一个

```python
import ccxt
from ccxt_passward import BinanceKey


exchange = ccxt.binance(
    {
    'apiKey': BinanceKey.apikey.value,
    'secret': BinanceKey.secret.value,
    }
)

symbol = "BTC/USDT"
# 最近一段时间内特定交易市场的统计信息, 通常使用24小时进行统
# exchange.fetch_tickers()  # for all tickers at once
# exchange.fetch_tickers(['ETH/BTC', 'LTC/BTC']) # listed tickers indexed by their symbols
ticker = exchange.fetch_ticker(symbol, params = {})   # for one ticker
print(ticker)

if (exchange.has['fetchOHLCV']):
    print(exchange.fetch_ohlcv('BTC/USDT', timeframe='1d', limit=10))

# 交易所时间
exchange_time = exchange.fetch_time()
print(exchange_time)

# 单一货币借贷利率
borrow = exchange.fetch_borrow_rate("BTC")
print(borrow)

# 资金费率历史
rate = exchange.fetch_funding_rate_history("BTC/USDT")
print(rate)

# 获取账户余额
balance = exchange.fetch_balance()
print(balance)
```
