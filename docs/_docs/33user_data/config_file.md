## 可交易余额
```python
tradable_balance_ratio = 0.5
```
1. 假设钱包中有10个ETH, 那么机器人最多能使用5ETH交易
2. 同一账户运行多个策略不建议使用此设置

## 交易初始额度
```python
available_capital=5000
```
1. 同一账户为每个机器人设置起始金额为5000
2. 此选项将替换tradable_balance_ratio

##
```python
max_open_trades = 3
```
最多开仓3笔交易

## 静态质押金额
```python
stake_amount = 50
```
1. 每笔开仓会质押50USDT
2. 假设`max_open_trades=3`, 则该机器人最多使用资金为: 3*50=150USDT

## 动态质押金额
```python
stake_amount="unlimited"
tradable_balance_ratio=0.99     # 建议设置项
max_open_trades = 3
```
动态质押金会随着账户金额变动对每笔开仓的质押金做调整, 假设账户有余额100, 则每笔开仓质押金为: 100*0.99/3 = 33

## 启用订单簿的入场价/离场价
```python
entry_pricing.use_order_book=True       # 是否启用订单簿
entry_pricing.order_book_top=1          # 使用买1卖1价, 也可以使用2, 3 等

exit_pricing.use_order_book=True
exit_pricing.order_book_top=2
```

## 不启用订单簿的入场价
```python
entry_pricing.use_order_book=False
entry_pricing.price_side="some"         # 总使用买一卖一价格
entry_pricing.price_last_balance
```
逻辑较复杂
1. entry_pricing.price_side
- ask: 总使用 ask1
- bid: 总使用 bid1
- other: 买用bid1, 卖用ask1
- same: 买用ask1, 卖用bid1

## 配置市价单
```python
  "order_types": {
    "entry": "market",
    "exit": "market"
    ...
  },
  "entry_pricing": {
    "price_side": "other",
    ...
  },
  "exit_pricing":{
    "price_side": "other",
    ...
  },
```

## 检查市场深度
```python
entry_pricing.check_depth_of_market.enabled=True
entry_pricing.check_depth_of_market.bids_to_ask_delta=1.1
```
1. 启用检查市场深度时, 将会根据订单簿深度过滤入场信号
2. 用订单簿买方总金额除以卖方总金额并将结果与`entry_pricing.check_depth_of_market.bids_to_ask_delta`比较, 只有当增量大于等于配置时才会挂单

## 外部控制交易指令
```python
force_entry_enable=False
```
是否可通过 Telegram 和 REST API 发送交易指令, 默认False, 谨慎开启

## K线内入场
```python
"ignore_buying_expired_candle_after": 300       # 提前5分钟入场
```
当使用较长周期(如1h)的策略时, 如果出现交易信号, 我们可能希望在1h的K线未走完前就入场

## 订单类型
```python
"order_types": {
    "entry": "limit",
    "exit": "limit",
    "emergency_exit": "market",
    "force_entry": "market",
    "force_exit": "market",
    "stoploss": "market",
    "stoploss_on_exchange": false,
    "stoploss_on_exchange_interval": 60
}
```
参考"config_strategy.md"文件

## 订单有效期
```python
"order_time_in_force": {
    "entry": "gtc",
    "exit": "gtc"
},
```
1. 不建议修改默认值
2. 有效期类型
- GTC: 直到用户取消
- FOK: 全部执行或取消
- IOC: 剩余部分取消

## 
```python

```
