---
title: step_backtesting
category: stepbystep
order: 1
---

## 启动命令
freqtrade backtesting --config config.json --strategy SampleStrategy

## 配置及初始化
1. 初始化配置: config = setup_optimize_configuration(args, RunMode.BACKTEST)
- INFO - Using config
- INFO - Verbosity set to 0
- INFO - Using max_open_trades
- INFO - Using user-data directory.
- INFO - Using data directory
- INFO - Parameter --cache=day detected ...
- INFO - Checking exchange...
- INFO - Exchange
- INFO - Using pairlist
- INFO - Validating configuration ...
2. 初始化 Backtesting
- INFO - Starting freqtrade in Backtesting mode
3. 加载交易所: self.exchange = ExchangeResolver.load_exchange
- INFO - Instance
- INFO - Using CCXT
- INFO - Applying additional ccxt config
- INFO - Using Exchange
- INFO - Using resolved exchange
4. 加载策略 StrategyResolver.load_strategy
    1. 递归加载策略 _load_strategy
    - INFO - Using resolved
    2. 策略校验 validate_strategy
    3. 通过配置更新策略参数
    - INFO - Found no parameter
    4. 更新此策略参数
    - INFO - Override ...
    - INFO - Strategy using ...
5. 策略配置校验: validate_config_consistency(config)
- INFO - Validating
6. 加载timeframe_detail: init_backtest_detail
7. 初始化 PairListManager
    1. 加载货币对 PairListResolver.load_pairlist
    - INFO - Using resolved pairlist
8. 初始化回测: self.init_backtest()
    1. 初始化回测数据 self.prepare_backtest(False)
    2. 初始化 Wallets(不输出日志)
    3. 初始化 BTProgress(回测进度)

## 启动回测
1. 加载回测数据: data, timerange = self.load_bt_data()
- INFO - Using indicator startup
- INFO - Loading data from
- WARNING - Moving start-date
2. 加载回测detail数据: self.load_bt_data_detail()
- INFO - Dataload complete.
3. 加载之前的回测结果: load_prior_backtest()
- WARNING - Backtest result caching disabled
4. 回测策略: self.backtest_one_strategy
- INFO - Running backtesting
    1. 启动策略: _set_strategy
        1. **策略回调: self.strategy.ft_bot_start()**
        - INFO - No params for
        - INFO - Strategy Parameter(default)...
        2. **策略回调: self.strategy.bot_loop_start**
    2. **策略回调: self.strategy.advise_all_indicators**
        1. **策略回调: self.populate_indicators**
    3. 调整启动周期: trim_dataframes(preprocessed, timerange, self.required_startup)
        1. **策略回调: strat.startup_candle_count**
    - INFO - Backtesting with data from
    4. 执行回测并获取结果: results = self.backtest
        1. 准备回测
        2. 更新wallets
        3. 将所有数据转为列表: _get_ohlcv_as_lists
            1. **策略回调: self.strategy.advise_entry**
                1. **策略回调: self.populate_entry_trend**
            2. **策略回调: self.strategy.advise_exit**
                1. **策略回调: self.populate_exit_trend**
            3. 删除预加载数据量(策略中填写): startup_candle_count
            4. 将 entry/exit 信号下移1位, 避免未来函数
            5. 以列表形式返回数据(只返回 HEADERS 定义列)
                
            
        4. 遍历数据回测
5. 整理回测结果: generate_backtest_stats
- INFO - dumping json to
6. 展示回测结果: show_backtest_results