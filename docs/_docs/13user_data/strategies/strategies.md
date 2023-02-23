## BreakEven
保住利润的同时尽快平仓. 该策略只是简单设置`minimal_roi`, 其他未作任何处理.

## CustomStoplossWithPSAR
1. use_custom_stoploss = True
2. custom_stoploss: 默认返回1, 回测模式单独处理
3. populate_indicators: 计算sar指标值
4. populate_entry_trend: 当前sar小于前一个sar, exit_long=1
5. populate_exit_trend: 所有exit_long=0, 即不必主动退出

## FixedRiskRewardLoss
1. use_custom_stoploss = True
2. custom_stoploss
- 获取开仓时间位置
- 获取开仓时对应的 stoploss_rate
- 计算默认止损比率: stoploss_rate/current_rate - 1
- 计算3倍止盈比率
- 计算1倍止盈触发价
- 止损默认是: close - 2*atr
- 价格超过1倍止盈价, 止损为: 开仓价 + 开平仓费用
- 价格超过3倍止盈价, 止损为: 3倍止盈价
3. populate_indicators: 计算atr, close-2*atr
4. populate_entry_trend: 所有enter_long=1, 即随时入场做多
5. populate_exit_trend: 所有exit_long=0, 即随时入场做空

## MyAwesomeStrategy 1
[参数优化示例: MyAwesomeStrategy 1](https://www.freqtrade.io/en/stable/hyperopt/)
1. populate_indicators添加指标: adx, rsi, macd, boll
2. 添加参数
- buy_adx: 默认30.1, 步长0.1(decimals=1)
- buy_rsi: 默认30, 默认步长1
- buy_adx_enabled: 布尔值, 默认True
- buy_rsi_enabled: 自定义选项, 默认False
- buy_trigger: 自定义选项, 默认"bb_lower"
3. populate_entry_trend
- 如果buy_adx_enabled为True, 则conditions添加buy_adx比较条件
- 如果buy_rsi_enabled为True, 则conditions添加buy_rsi比较条件
- 如果buy_trigger == `bb_lower`, 则conditions添加bb_lowerband指标比较条件
- 如果buy_trigger == `macd_cross_signal`, 则conditions添加macd指标比较条件
- conditions添加volume比较条件
- conditions中所有条件都满足的`enter_long` = 1

## MyAwesomeStrategy 2
[参数优化示例: MyAwesomeStrategy 2](https://www.freqtrade.io/en/stable/hyperopt/)
1. populate_indicators添加指标: 所有周期EMA均线
2. 添加参数
- buy_ema_short: 默认5, 短周期均线
- buy_ema_long: 默认200, 长周期均线
3. populate_entry_trend
- conditions添加短周期均线上穿长周期均线条件
- conditions添加volume比较条件
- conditions中所有条件都满足的`enter_long` = 1
4. populate_exit_trend
- conditions添加长周期均线上穿短周期均线条件
- conditions添加volume比较条件
- conditions中所有条件都满足的`exit_long` = 1

## Diamond
1. 添加参数: buy_params, sell_params等
2. 添加超参数: buy_vertical_push, buy_horizontal_push, buy_fast_key, buy_slow_key等
3. populate_indicators: 添加需要用的技术指标
4. populate_entry_trend: 快速均线偏移N期后上穿慢速均线的M倍
5. populate_exit_trend: 快速均线偏移N期后下穿慢速均线的M倍

## Heracles
1. 添加参数: buy_params, sell_params等
2. 添加超参数: buy_div_min, buy_div_max, buy_indicator_shift, buy_crossed_indicator_shift
3. populate_indicators: 利用ta包指标
4. populate_entry_trend: 指标变形然后添加条件筛选
5. populate_exit_trend: 不做处理

## hlhb
1. populate_indicators: 计算指标
2. populate_entry_trend: 根据指标判断入场
3. populate_exit_trend: 根据指标判断离场

## HourBasedStrategy
1. 添加参数: buy_params, sell_params等
2. 添加超参数: buy_hour_min, buy_hour_max, sell_hour_min, sell_hour_max
3. populate_indicators: 添加小时
4. populate_entry_trend: 某两个小时之间买入
5. populate_exit_trend: 某两个小时之间卖出

## InformativeSample
利用`merge_informative_pair`函数可以编写跨周期策略
1. 设置 `timeframe = '5m'`
2. informative_pairs: 添加15m数据
3. populate_indicators
- 计算技术指标
- 获取15m价格数据
- 计算基于15m的技术指标
- 使用`merge_informative_pair`函数合并数据
4. populate_entry_trend: 入场条件判断
5. populate_exit_trend: 离场条件判断

## multi_tf
主要利用装饰器添加跨周期指标
1. @informative('1h'): 生成`1h`周期数据
2. @informative('1h', 'ETH/BTC'): 对ETH/BTC货币对生成`1h`周期数据
3. @informative('1h', 'BTC/{stake}', 'BTC_{column}_{timeframe}'): 自定义列名
4. @informative('1h', 'BTC/{stake}')
- stake是报价货币
5. @informative('1h', 'BTC/{stake}', '{base}_{column}_{timeframe}')
- base: 基础货币
6. populate_entry_trend: 添加入场条件
7. populate_exit_trend: 添加离场条件

## PatternRecognition
形态识别策略

# 期货策略

## FAdxSmaStrategy
1. 一般设置
- `can_short = True`
- `process_only_new_candles = False`
- ``
2. 添加参数
- pos_entry_adx
- pos_exit_adx
3. populate_indicators: 计算多个指标
4. populate_entry_trend: 分别添加多头和空头入场条件
5. populate_exit_trend: 分别添加多头和空头离场条件

## FOttStrategy
1. 自定义指标计算公式ott: ott和var
2. populate_indicators: 计算指标
3. populate_entry_trend: 添加入场条件
4. populate_exit_trend: 添加离场条件

## 