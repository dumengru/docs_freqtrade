## DataProvider
1. add_pairlisthandler: 初始化后添加货币对
2. ohlcv: 从交易所获取bar数据
3. historic_ohlcv: 从缓存获取历史数据(回测)
4. get_pair_dataframe: ohlcv和historic_ohlcv结合
5. get_analyzed_dataframe: 
6. runmode: 获取回测模式
7. current_whitelist: 获取货币白名单, 当设置动态货币列表时很有用
8. clear_cache: 清空缓存
9. refresh: 刷新数据
10. available_pairs: 获取有缓存数据的货币对
11. market: 获取货币对的market信息
12. ticker: 获取该货币对最新ticker数据
13. orderbook: 获取货币对最新订单簿数据
14. send_msg: 发送msg到rpc

## Helper functions
1. merge_informative_pair: 合并数据
2. stoploss_from_open: 返回相对于开盘价的止损值
3. stoploss_from_absolute: 返回绝对止损价格

## Additional data (Wallets)
获取钱包数据
```python
if self.wallets:
    free_eth = self.wallets.get_free('ETH')
    used_eth = self.wallets.get_used('ETH')
    total_eth = self.wallets.get_total('ETH')
```

## Additional data (Trades)
获取交易数据
```python

```

## 锁定交易
```python
self.lock_pair(pair, until, [reason])

self.unlock_pair(pair)          # 解锁货币对
self.unlock_reason(<reason>)    # 解锁某原因锁定的货币对
```
参数:
- pair: 货币对名称
- until: 将来的某个时间datetime
- reason: 锁定原因


## 函数回调顺序
#### 回测
1. bot_start
2. bot_loop_start
3. populate_indicators
4. populate_entry_trend
5. populate_exit_trend
6. custom_entry_price
7. leverage
8. custom_stake_amount
9. confirm_trade_entry
10. custom_stoploss
11. custom_exit
12. custom_stoploss
13. custom_exit
14. custom_stoploss
15. custom_exit
16. ...
17. custom_entry_price
18. leverage
19. custom_stake_amount
20. confirm_trade_entry
21. custom_stoploss
22. custom_exit
23. custom_stoploss
24. custom_exit
25. custom_stoploss
26. custom_exit
27. ...
28. confirm_trade_exit
29. custom_exit
30. custom_exit
31. custom_exit
32. ...
33. custom_exit_price
34. confirm_trade_exit

#### 实盘
1. informative_pairs
2. bot_loop_start
3. populate_indicators
4. populate_entry_trend
5. populate_exit_trend
6. informative_pairs
7. bot_loop_start
8. informative_pairs
9. bot_loop_start
10. informative_pairs
11. bot_loop_start
12. ...

## 回调函数
#### bot_loop_start
1. 回调频率, 5s/次
2. 可用于执行计算, 加载外部数据等

## 参数优化
1. 待优化参数需要定义 `space=buy` 和 `space=sell` 对应买入和卖出信号, 或者命名形式为 buy_* 和 sell_*
2. 所有参数优化需要用到的指标值都必须在`populate_indicators`中先计算
3. protection参数需要定义 `space="protection", optimize=True`
4. 优化目标包括: 
- ShortTradeDurHyperOptLoss
- OnlyProfitHyperOptLoss
- SharpeHyperOptLoss
- SharpeHyperOptLossDaily
- SortinoHyperOptLoss
- SortinoHyperOptLossDaily
- CalmarHyperOptLoss
- MaxDrawDownHyperOptLoss
- MaxDrawDownRelativeHyperOptLoss
- ProfitDrawDownHyperOptLoss
3. 参数类型
- IntParameter
- DecimalParameter
- RealParameter
- CategoricalParameter
- BooleanParameter
4. 优化空间
- all
- buy
- sell
- roi
- stoploss
- trailing
- protection
- default: (all, 但不包含 trailing 和 protection)

## Protections
1. 风控类型
- StoplossGuard: 如果在特定时间窗口内发生一定数量的止损, 则停止交易
- MaxDrawdown: 如果达到最大回撤, 则停止交易
- LowProfitPairs: 锁定利润低的对
- CooldownPeriod: 不要在卖出交易后立即进入交易
2. 通用设置
- method: 风控类型
- stop_duration_candles: 锁定蜡烛数量
- stop_duration: 锁定分钟时间(不能与stop_duration_candles一起使用)
- lookback_period_candles: 只考虑最后`lookback_period_candles`根蜡烛
- lookback_period: 只考虑最后`lookback_period`分钟
- trade_limit: 最低交易数量
1. StoplossGuard 配置
- only_per_pair: 每次只查看一种货币对
- only_per_side: 仅锁定一侧(如锁定做多后仍可开空)
4. MaxDrawdown 配置
- max_allowed_drawdown: 最大允许回撤
5. LowProfitPairs 配置
- required_profit: 要求的最低利润率
- only_per_pair: 每次只查看一种货币对
6. 所有风控可以随意组合

## 多周期策略
1. 利用`merge_informative_pair`函数手动处理跨周期数据
2. 利用`informative`装饰器添加跨周期数据
- @informative('1h'): 生成1h周期数据
- @informative('1h', 'ETH/BTC'): 对ETH/BTC货币对生成1h周期数据
- @informative('1h', 'BTC/{stake}', 'BTC_{column}_{timeframe}'): 自定义列名
- @informative('1h', 'BTC/{stake}')
    - stake是报价货币
- @informative('1h', 'BTC/{stake}', '{base}_{column}_{timeframe}')
    - base: 基础货币

