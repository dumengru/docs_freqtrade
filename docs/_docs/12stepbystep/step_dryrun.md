---
title: step_dryrun
category: stepbystep
order: 2
---

## 启动命令
freqtrade trade --config config.json --strategy SampleStrategy

## 配置及初始化
1. 初始化 Worker
- INFO - Starting worker
2. 执行 Worker._init
3. 获取配置: 初始化 Configuration
4. 执行 Configuration.get_config
5. 执行 Configuration.load_config
6. 从命令行获取config文件相对路径
7. 加载配置文件: 执行 load_from_files
- INFO - Using config:
8. 初始化配置
    1. 更新config配置:internals
    2. 更新config配置:pairlists
    3. 更新config配置:original_config
    4. 配置日志: self._process_logging_options(config)
        1. 更新config配置: verbosity
        2. 更新config配置: logfile
        3. 设置日志配置
        - INFO - Verbosity
    5. 配置运行模式: self._process_runmode(config)
        1. 更新config配置: dry_run
        2. 设置运行模式
        - INFO - Runmode
        3. 更新config配置: runmode
    6. 更新公共项配置: self._process_common_options(config)
        1. 更新config配置: strategy
        2. 更新config配置: strategy_path
        3. 更新config配置: db_url
        4. 更新config配置: db_url_from
        3. 更新config配置: internals.sd_notify
    7. 配置交易项: self._process_trading_options(config)
    - INFO - Dry run
        1. 配置默认 db_url
    - INFO - Using DB
    8. 更新参数优化项配置: self._process_optimize_options(config)
        1. 更新config配置: timeframe
        2. 更新config配置: position_stacking
        3. 更新config配置: enable_protections
        4. 更新config配置: use_max_market_positions/max_open_trades
        - INFO - Using max_open_trades
        5. 更新config配置: max_open_trades
        6. 更新config配置: timeframe_detail
        7. 更新config配置: backtest_show_pair_list
        8. 更新config配置: stake_amount
        9. 更新config配置: dry_run_wallet
        10. 更新config配置: fee
        11. 更新config配置: timerange
        ...
        - INFO - Using user-data directory
        - INFO - Using data directory
    9. 更新绘图项配置: self._process_plot_options(config)
    10. 更新数据项配置: self._process_data_options(config)
    11. 更新数据分析项配置: self._process_analyze_options(config)
    12. 更新ai项配置: self._process_freqai_options(config)
    13. 检查交易所配置是否正常
    - INFO - Checking exchange
    - INFO - Exchange
    14. 配置pairlist
    - INFO - Using pairlist
    15. 更新/移除旧版本配置
9. 初始化 FreqtradeBot
- INFO - Starting freqtrade
10. 初始化 StrategyResolver
11. 加载策略 StrategyResolver.load_strategy
    1. 递归加载策略 _load_strategy
    - INFO - Using resolved
    2. 策略校验 validate_strategy
    3. 通过配置更新策略参数
    - INFO - Found no parameter
    4. 更新此策略参数
    - INFO - Override ...
    - INFO - Strategy using ...
12. 策略配置校验: validate_config_consistency(config)
- INFO - Validating
13. 初始化 ExchangeResolver
14. 加载交易所 ExchangeResolver.load_exchange
15. 初始化交易所对象 ExchangeResolver._load_exchange
- INFO - Instance
- INFO - Using CCXT
- INFO - Applying additional ccxt config
- INFO - Using Exchange
- INFO - Using resolved exchange
16. 初始化 Wallets 
- INFO - Wallets
17. 初始化 RPCManager
- INFO - Enabling rpc.telegram
- INFO - Enabling rpc.api_server
18. 初始化 ApiServer
- INFO - Starting HTTP Server
- INFO - Starting Local Rest Server
- INFO - Started server process
19. 初始化 PairListManager
    1. 加载货币对 PairListResolver.load_pairlist
    - INFO - Using resolved pairlist
20. 初始化 DataProvider
21. 初始化 Edge
22. **策略回调:self.strategy.ft_bot_start()**
    1. 加载ai模型
    2. 加载超参数
    - INFO - No params for
    - INFO - Strategy Parameter(default)...
23. 初始化 ProtectionManager
- INFO - No params for protection found
- INFO - No protection Handlers defined

## 工作主流程
1. 启动Worker: Worker.run() 
2. state != old_state
- INFO - Changing state to: RUNNING
    1. 启动Bot: self.freqtrade.startup()
    - INFO - Sending rpc message
    2. 反向填充交易合约相关数据: self.startup_backpopulate_precision()
        1. 查询Trade.contract_size.is_(None)的Trade
        2. 填充数据
            1. trade.precision_mode
            2. trade.amount_precision
            3. trade.price_precision
            4. trade.contract_size 
        3. 提交修改: Trade.commit()
    3.如果配置了 edge, 调整 trade.adjust_stop_loss
    4. 更新已有持仓的交易数据 self.startup_update_open_orders()
3. state == State.RUNNING
    1. 运行Bot: self.freqtrade.process()
    2. 加载市场数据: self.exchange.reload_markets()
    3. 更新已有持仓的trades: update_closed_trades_without_assigned_fees
    4. 获取持仓的trades并更新白名单
    5. 更新最新的K线数据到数据库
    6. 策略回调: **self.strategy.bot_loop_start**
    7. 将数据转为DataFrame格式: **self.strategy.analyze(self.active_pair_whitelist)**
    8. 管理订单: self.manage_open_orders()
    9. 管理成交: self.exit_positions(trades)
    10. 调整持仓: self.process_open_trade_positions()
    11. 执行成交: self.enter_positions()
    12. 提交Trade
    13. 发送信息并更新时间
4. state == State.STOPPED
    1. 停止Bot: self.freqtrade.process_stopped()
        1. 如果配置了 cancel_open_orders_on_exit, 则撤销全部订单
- INFO - Cleaning up modules ...
- INFO - Cleaning up rpc modules ...
- INFO - Cleaning up rpc.apiserver ...
- INFO - Stopping API Server

#### 策略回调
self.strategy.adjust_trade_position
self.strategy.custom_entry_price
self.strategy.leverage
self.strategy.custom_stake_amount
self.strategy.adjust_entry_price
self.strategy.custom_exit_price
self.strategy.confirm_trade_exit
self.strategy.confirm_trade_entry