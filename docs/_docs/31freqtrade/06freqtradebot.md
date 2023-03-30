---
title: freqtradebot.py
category: freqtrade
order: 6
---

## FreqtradeBot

#### __init__初始化
- 传入config
1. 白名单: self.active_pair_whitelist
2. 设置状态: self.state = State.STOPPED
3. 加载策略: self.strategy
4. 检查配置: validate_config_consistency(config)
5. 加载交易所: self.exchange
6. 初始化数据库: init_db
7. 创建钱包对象: self.wallets
8. 加载时间框架: PairLocks.timeframe
9. 创建rpc管理器对象: self.rpc
10. 创建货币对对象: self.pairlists
11. 创建数据管理器: self.dataprovider
12. 将数据管理器添加到策略中: self.strategy.dp = self.dataprovider
13. 将钱包对象添加到策略中: self.strategy.wallets = self.wallets
14. 添加edge对象: self.edge
15. 更新白名单: self.active_pair_whitelist
16. 更新状态: self.state
17. 创建线程锁对象: self._exit_lock
18. 初始化日志对象: LoggingMixin.__init__()
19. 设置交易模式: self.trading_mode
20. 创建调度器对象: self._schedule
21. 判断交易模式: self.trading_mode
22. **启动交易策略: self.strategy.ft_bot_start()**
23. 创建保护管理器对象: self.protections

#### startup()
1. 启动rpc管理器
2. (#待定#)self.startup_backpopulate_precision()
3. **调整止损: Trade.stoploss_reinitialization(self.strategy.stoploss)**
4. 启动开仓: self.startup_update_open_orders()

#### process_stopped()
- 停止bot
1. 如果配置了"cancel_open_orders_on_exit", 则撤销全部订单

#### process()
1. 加载市场: self.exchange.reload_markets()
2. (#待定#)self.update_closed_trades_without_assigned_fees()
3. 查询交易: trades = Trade.get_open_trades()
4. 更新白名单: self.active_pair_whitelist = self._refresh_active_whitelist(trades)
5. 根据数据管理器: self.dataprovider.refresh()
6. **回调策略: self.strategy.bot_loop_start**
7. **策略分析: self.strategy.analyze(self.active_pair_whitelist)**
8. 加锁管理开仓订单: self.manage_open_orders()
9. 加锁管理订单成交: self.exit_positions(trades)
10. 加锁管理持仓: self.process_open_trade_positions()
11. 成交: self.enter_positions()
12. 提交操作: Trade.commit()
13. 推送消息: self.rpc.process_msg_queue()
14. 更新时间: self.last_process

#### check_for_open_trades()
- 检查开仓交易

#### _refresh_active_whitelist()


#### self.startup_backpopulate_precision()

#### self.startup_update_open_orders()

#### self.cancel_all_open_orders()
- 撤销全部订单

#### self.update_closed_trades_without_assigned_fees()

#### manage_open_orders()

#### self.exit_positions()

#### self.process_open_trade_positions()

#### self.enter_positions()
