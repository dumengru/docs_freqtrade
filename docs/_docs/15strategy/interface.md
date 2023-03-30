---
title: interface.py
category: strategy
order: 1
---

## IStrategy

#### 类属性

1. 参数文件: _ft_params_from_file: Dict
2. 最低回报率: minimal_roi: Dict = {}
3. 止损: stoploss
4. 是否可做空: can_short: bool = False
5. 时间框架: timeframe
6. 生效时间: order_time_in_force
7. 只在出现新K时执行"populate_indicators": process_only_new_candles
8. 策略启动时需要的K线数量: startup_candle_count
9. 绘图配置
10. use_custom_stoploss: 是否启用"custom_stoploss"
11. use_exit_signal: 是否启用"custom_exit"
12. position_adjustment_enable： 是否启用"adjust_trade_position"
13. dp: 提供数据DataProvider

#### 策略回调方法

1. populate_indicators: 计算所有的技术指标
2. populate_entry_trend: 根据指标计算入场信号
3. populate_exit_trend: 根据指标计算离场信号
4. bot_start: 策略实例化后启动
5. bot_loop_start: 策略开始时启动循环
6. check_entry_timeout: 定义入场订单超时规则
7. check_exit_timeout: 定义立场订单超时规则
8. confirm_trade_entry: 交易入场确认
9. confirm_trade_exit: 交易离场确认
10. custom_stoploss: 自定义止损比率
11. custom_exit: 自定义退出规则
12. custom_entry_price: 自定义入场价格逻辑
13. custom_exit_price: 自定义离场价格逻辑
14. custom_stake_amount: 自定义下单量
15. adjust_trade_position: 调整仓位
16. adjust_entry_price: 调整入场价
17. leverage: 为交易自定义杠杆
18. informative_pairs: 添加其他时间框架数据
19. populate_any_indicators: 为ai模型添加特征
