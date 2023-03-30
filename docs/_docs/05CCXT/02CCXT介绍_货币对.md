---
title: CCXT介绍_货币对
category: CCXT
order: 2
---

## 货币模型

```json
{
    'id':       'btc',       // string literal for referencing within an exchange
    'code':     'BTC',       // uppercase unified string literal code the currency
    'name':     'Bitcoin',   // string, human-readable name, if specified
    'active':    true,       // boolean, currency status (tradeable and withdrawable)
    'fee':       0.123,      // withdrawal fee, flat
    'precision': 8,          // number of decimal digits "after the dot" (depends on exchange.precisionMode)
    'deposit':   true        // boolean, deposits are available
    'withdraw':  true        // boolean, withdraws are available
    'limits': {              // value limits when placing orders on this market
        'amount': {
            'min': 0.01,     // order amount should be > min
            'max': 1000,     // order amount should be < max
        },
        'withdraw': { ... }, // withdrawal limits
        'deposit': {...},
    },
    'networks': {...}        // network structures indexed by unified network identifiers (ERC20, TRC20, BSC, etc)
    'info': { ... },         // the original unparsed currency info from the exchange
}
```
- id：市场ID，用来在交易所内区分不同市场的字符串或数字ID

- symbol：市场符号，用来表示交易对的大写的字符串代码。通常记作：基础货币/报价货币，例如： BTC/USD, LTC/CNY 或 ETH/EUR等等。在ccxt库中使用市场符号来引用不同的市场

- base：基础货币代码，表示基础法币或加密货币的统一的大写字符串代码，代码是标准化的，在 CCXT中以及CCXT统一API中，使用货币代码来引用不同的货币

- quote：报价货币代码，用来表示报价法币或数字货币的统一的大写字符串代码

- baseId：基础货币ID，是交易所特定的表示基础货币的ID，不是统一的代码，理论上可以是任何 字符串

- quoteId：报价货币ID，是交易所特定的表示报价货币的ID，也不是统一的代码，每个交易所自行维护
- active：是否激活，布尔值，表示这个市场是否可交易。通常如果一个市场未激活，那么所有相关的 行情、委托账本以及其他访问端结点都返回空响应、全零、无数据或过时数据。调用者需要检查市场 是否激活并且定期刷新市场缓存

- info：一个用于记录非共性市场属性的关联数组，包括手续费、费率、限制以及其他一般性市场信息。 内部的info数组对每个特定的市场都是不同的，其内容依赖于交易所

- precision：在委托单中金额相关字段（例如价格、数量、成本等）支持的最大小数位数

- limits：限值，价格、数量和成本等的最高和最低限值，其中：成本 = 价格 * 数量

## 简单使用

`exchange.load_markets`方法中包含了交易所所有的货币对信息, 建议熟悉相关内容

```python
import asyncio
import ccxt

# ccxt_passward是我保存自己apiKey的文件
from ccxt_passward import BinanceKey


exchange = ccxt.binance(
    {
    'apiKey': BinanceKey.apikey.value,
    'secret': BinanceKey.secret.value,
    }
)

# 获取所有货币信息
markets = exchange.load_markets()

print("支持的货币对: ", markets.keys())

symbol = "BTC/USDT"
print(f"{symbol}信息: ", markets[symbol].keys())
print(f"{symbol}具体信息: ", markets[symbol])

print(f"{symbol}下单量限制: ", markets[symbol]['limits']['amount'])
print(f"{symbol}下单价限制: ", markets[symbol]['limits']['price'])
print(f"{symbol}数据精度: ", markets[symbol]['precision'])
```

## 精度和限值

加密货币市场的价格并非十分规范, 价格高的货币如BTC每个售价超过20000USDT, 价格低的货币如SHIB每个售价不足0.0001USDT, 因此在下单交易时对每个货币对的价格和数量都会有限制.

- 精度: 即下单价格或委托数量的小数位数
- 限制: 即下单价格或委托数量的区间范围

1. 委托单量 >=0.05, 精度最高4位小数

```python
(market['limits']['amount']['min'] == 0.05) && (market['precision']['amount'] == 4)
```

2. 价格应当 >= 0.019, 价格精度最高5位小数

```python
(market['limits']['price']['min'] == 0.0019) && (market['precision']['price'] == 5)
```

3. 数量应当 >= 50, 精度为负数表示应当为10的倍数

```python
(market['limits']['amount']['min'] == 50) && (market['precision']['amount'] == -1)
```

## 委托单限制

以下是委托单的一些限制, 如果交易某货币的下单不满足以下限制则会发单失败

1. 委托单amount > limits['min']['amount']
2. 委托单amount < limits['max']['amount']
3. 委托单price > limits['min']['price']
4. 委托单price < limits['max']['price']
5. 委托单cost (amount * price) > limits['min']['cost']
6. 委托单cost (amount * price) < limits['max']['cost']
7. amount的精度 <= precision['amount']
8. price 的精度 <= precision['price']

## 数据精度

1. decimal_to_precision

CCXT提供了`decimal_to_precision`方法可以方便的设置合理价格

```python
decimal_to_precision(n, rounding_mode=ROUND, precision=None, counting_mode=DECIMAL_PLACES, padding_mode=NO_PADDING)
```

- rounding_mode, 取整模式：
    - ROUND: 取整精度要求之后的小数位
    - TRUNCATE: 截断精度要求之后的小数位

- precision: 数据精度, 整型, 可以是负数

- counting_mode, 计数模式: 
    - DECIMAL_PLACES: 统计所有的数字, 99%的交易所使用这种计数模式
    - SIGNIFICANT_DIGITS: 仅统计非零数字, 有些交易所(bitfinex等)采用这种模式的计数
    - TICK_SIZE: 有些交易所只允许某个特定值的整数倍(bitmex使用这种模式)

- padding_mode, 填充模式包括:
    - NO_PADDING: 无填充, 大多数情况下的默认模式
    - PAD_WITH_ZERO: 使用0字符填充至精度要求

```
from ccxt.base.decimal_to_precision import decimal_to_precision
from ccxt.base.decimal_to_precision import TRUNCATE              
from ccxt.base.decimal_to_precision import ROUND                 
from ccxt.base.decimal_to_precision import DECIMAL_PLACES        
from ccxt.base.decimal_to_precision import SIGNIFICANT_DIGITS    
from ccxt.base.decimal_to_precision import TICK_SIZE            
from ccxt.base.decimal_to_precision import PAD_WITH_ZERO         
from ccxt.base.decimal_to_precision import NO_PADDING           
from ccxt.base.decimal_to_precision import number_to_string     
from ccxt.base.exchange import Exchange                         
from ccxt.base.precise import Precise                           

assert decimal_to_precision('12.3456700', TRUNCATE, 100, DECIMAL_PLACES) == '12.34567'
assert decimal_to_precision('12.3456', TRUNCATE, 100, DECIMAL_PLACES) == '12.3456'
assert decimal_to_precision('12.3456', TRUNCATE, 4, DECIMAL_PLACES) == '12.3456'
assert decimal_to_precision('12.3456', TRUNCATE, 0, DECIMAL_PLACES) == '12'
assert decimal_to_precision('0.00000001', TRUNCATE, 8, DECIMAL_PLACES) == '0.00000001'
assert decimal_to_precision('0.000000000', TRUNCATE, 9, DECIMAL_PLACES, PAD_WITH_ZERO) == '0.000000000'
assert decimal_to_precision('12.3456', TRUNCATE, -1, DECIMAL_PLACES) == '10'
assert decimal_to_precision('-0.9', TRUNCATE, 0, DECIMAL_PLACES) == '0'
```

参考示例: https://github.com/ccxt/ccxt/blob/master/python/ccxt/test/test_decimal_to_precision.py

## ..._to_precision

除了`decimal_to_precision`函数, 以下几个方法可以根据交易货币对更加方便设置数据精度

```python
- def cost_to_precision(self, symbol, cost)
- def price_to_precision(self, symbol, price)
- def amount_to_precision(self, symbol, amount)
- def fee_to_precision(self, symbol, fee)
- def currency_to_precision(self, code, fee, networkCode=None)
```

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

markets = exchange.load_markets()

symbol = 'BTC/USDT'
amount = 1.2345678  # amount in base currency BTC
price = 87654.321  # price in quote currency USDT
formatted_amount = exchange.amount_to_precision(symbol, amount)
formatted_price = exchange.price_to_precision(symbol, price)
print(f"{symbol}数据精度: ", markets[symbol]['precision'])
print(symbol, " 数量: ", formatted_amount, " 价格: ", formatted_price)

symbol = 'SHIB/USDT'
amount = 12.345678  # amount in base currency SHIB
price = 0.87654321  # price in quote currency USDT
formatted_amount = exchange.amount_to_precision(symbol, amount)
formatted_price = exchange.price_to_precision(symbol, price)
print(f"{symbol}数据精度: ", markets[symbol]['precision'])
print(symbol, " 数量: ", formatted_amount, " 价格: ", formatted_price)
```

