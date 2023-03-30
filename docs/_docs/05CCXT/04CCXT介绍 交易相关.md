---
title: CCXT介绍_交易相关
category: CCXT
order: 4
---

## 查询交易

- fetchTrades(symbol, since = undefined, limit = undefined, params = {})
- fetchMyTrades()
- fetchOrderTrades()

```json
{
  'info':         { ... },                    // the original decoded JSON as is
  'id':           '12345-67890:09876/54321',  // string trade id
  'timestamp':    1502962946216,              // Unix timestamp in milliseconds
  'datetime':     '2017-08-17 12:42:48.000',  // ISO8601 datetime with milliseconds
  'symbol':       'ETH/BTC',                  // symbol
  'order':        '12345-67890:09876/54321',  // string order id or undefined/None/null
  'type':         'limit',                    // order type, 'market', 'limit' or undefined/None/null
  'side':         'buy',                      // direction of the trade, 'buy' or 'sell'
  'takerOrMaker': 'taker',                    // string, 'taker' or 'maker'
  'price':        0.06917684,                 // float price in quote currency
  'amount':       1.5,                        // amount of base currency
  'cost':         0.10376526,                 // total cost, `price * amount`,
  'fee':          {                           // provided by exchange or calculated by ccxt
      'cost':  0.0015,                        // float
      'currency': 'ETH',                      // usually base currency for buys, quote currency for sells
      'rate': 0.002,                          // the fee rate (if available)
  },
}
```

## 查询账户余额

- fetchBalance (params={})

```json
{
    'info':  { ... },    // the original untouched non-parsed reply with details
    'free':  {           // money, available for trading, by currency
        'BTC': 321.00,   // floats...
        'USD': 123.00,
        ...
    },
    'used':  { ... }
    'total': { ... },    // total (free + used), by currency
    'BTC':   {           // string, three-letter currency code, uppercase
        'free': 321.00   // float, money available for trading
        'used': 234.00,  // float, money on hold, locked, frozen or pending
        'total': 555.00, // float, total balance (free + used)
    },
    'USD':   {           // ...
        'free': 123.00   // ...
        'used': 456.00,
        'total': 579.00,
    },
    ...
}
```

## 查询订单

- fetchCanceledOrders()
- fetchClosedOrder()
- fetchClosedOrders()
- fetchOpenOrder()
- fetchOpenOrders()
- fetchOrder()
- fetchOrders()
    - 通过`exchange.has`查看方法是否可用

```json
{
  'id':                '12345-67890:09876/54321', // string
  'clientOrderId':     'abcdef-ghijklmnop-qrstuvwxyz', // a user-defined clientOrderId, if any
  'datetime':          '2017-08-17 12:42:48.000', // ISO8601 datetime of 'timestamp' with milliseconds
  'timestamp':          1502962946216, // order placing/opening Unix timestamp in milliseconds
  'lastTradeTimestamp': 1502962956216, // Unix timestamp of the most recent trade on this order
  'status':      'open',        // 'open', 'closed', 'canceled', 'expired', 'rejected'
  'symbol':      'ETH/BTC',     // symbol
  'type':        'limit',       // 'market', 'limit'
  'timeInForce': 'GTC',         // 'GTC', 'IOC', 'FOK', 'PO'
  'side':        'buy',         // 'buy', 'sell'
  'price':        0.06917684,   // float price in quote currency (may be empty for market orders)
  'average':      0.06917684,   // float average filling price
  'amount':       1.5,          // ordered amount of base currency
  'filled':       1.1,          // filled amount of base currency
  'remaining':    0.4,          // remaining amount to fill
  'cost':         0.076094524,  // 'filled' * 'price' (filling price used where available)
  'trades':     [ ... ],        // a list of order trades/executions
  'fee': {                      // fee info, if available
      'currency': 'BTC',        // which currency the fee is (usually quote)
      'cost': 0.0009,           // the fee amount in that currency
      'rate': 0.002,            // the fee rate (if available)
  },
  'info': { ... },              // the original unparsed order structure as is
}
```

- fee：手续费信息这部分的工作还在进行中，取决于交易所提供的接口，这部分信息可能不完整甚至完全没有

- fee.currency：手续费货币可能与所交易的货币都不一样。例如，一个ETH/BTC的委托单可能使用USD支付手续费

- lastTradeTimestamp：最后交易时间戳表示该委托单最后一次交易的时间。在有些情况下， 这个字段可能没有值或者是undefined/None/null，例如交易所不支持，或者委托单还是敞口状态

- status：委托单状态的优先级高于最后交易时间戳

- cost：委托单花费 = filled * price ，表示委托单的总花费，cost字段是处于方便目的而提供， 值可以根据其他字段推导出来


## 创建订单

**基础函数**

create_order(self, symbol, type, side, amount, price=None, params={})

- symbol：希望交易的市场对应的交易对符号，例如 BTC/USD, ZEC/ETH, DOGE/DASH, 等等。 请确保所制定的交易对符号在目标交易所存在并且可以交易

- type：委托单类型

- side：委托单的交易方向，买入还是卖出。当你下买单时，给出报价货币并将收到基准货币。 例如，买入BTC/USD意味着你将支出美元并收到比特币。当你卖出BTC/USD时则是相反的， 你将支出比特币并收到美元

- amount：你希望交易的数量。这通常指的是交易对符号中的基准货币的数量，虽然也有些 交易所会要求提供报价货币的数量，还有一些会根据委托单的方向而要求分别提供基准货币 或报价货币的数量

- price：你希望为交易支付的报价货币的数量，仅限于限价单

**特殊函数**

CCXT提供几种标准形式的订单方法

市价单
- exchange.create_market_sell_order(symbol, amount, params)
- exchange.create_market_buy_order(symbol, amount, params)

限价单
- exchange.create_limit_buy_order(symbol, amount, price, params)
- exchange.create_limit_sell_order(symbol, amount, price, params)

止损单/止盈单(params = {'stopLossPrice': price1, 'takeProfitPrice': price2})
- 从上方跌至`stopLossPrice`, 用于买单
- 从下方涨至`stopLossPrice`, 用于卖单

**计划委托**

触发单(当价格从任何方向触发`triggerPrice`时激活)
- 添加参数: params = {'triggerPrice': price}

**修改订单**

editOrder (id, symbol, type, side, amount, price = undefined, params = {})
    
**取消委托单**

cancelOrder(id, symbol, params)

cancelOrders (ids, symbol = undefined, params = {})

以下两种情况会出现`OrderNotFound`异常
- 取消一个已经完成的委托单
- 取消一个已经取消的委托单

## 手续费

**查询手续费**

手续费包括: 
1. 交易手续费：向交易所支付的交易手续费，通常按成交量的百分点计取
2. 资金操作手续费：在充值和提现时向交易所支付的费用，包含链上交易费用

- fetchFees (params = {})
- fetchTradingFees (params = {})
- fetchFundingFees (params = {})

```json
{
    'type': takerOrMaker,
    'currency': 'BTC', // the unified fee currency code
    'rate': percentage, // the fee rate, 0.05% = 0.0005, 1% = 0.01, ...
    'cost': feePaid, // the fee cost (amount * fee rate)
}
```

**预算交易费**

下单之前使用该方法预估一下交易费用总是明智的

- calculateFee (symbol, type, side, amount, price, takerOrMaker = 'taker', params = {})

## 示例

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

# 查询交易
if exchange.has['fetchTrades']:
    print(exchange.fetch_trades("BTC/USDT", limit=3))

# 查询个人历史交易
print(exchange.fetch_my_trades("BTC/USDT"))

# 查询余额
print(exchange.fetch_balance())

print(exchange.orders)

# 查询手续费
print(exchange.fetch_fees())
# 查询take/maker手续费
print(exchange.markets['BTC/USDT']['taker'])
print(exchange.markets['BTC/USDT']['maker'])
# 计算交易手续费
print(exchange.calculate_fee("BTC/USDT", "market", "long", 1, 20000, "taker"))

# 查看交易所状态
print(exchange.fetch_status())

# 预算交易费
print(exchange.calculate_fee("BTC/USDT", "market", "long", 1, 20000, "taker"))
```