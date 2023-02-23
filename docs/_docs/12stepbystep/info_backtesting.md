---
title: info_backtesting
category: stepbystep
order: 5
---

(env_crypto) D:\DuWork\github_project\freq_research>freqtrade backtesting --config config.json --strategy SampleStrategy

freqtrade.configuration.load_config - INFO - Using config: config.json ...
freqtrade.loggers - INFO - Verbosity set to 0
freqtrade.configuration.configuration - INFO - Using max_open_trades: 3 ...
freqtrade.configuration.configuration - INFO - Using user-data directory: user_data ...
freqtrade.configuration.configuration - INFO - Using data directory: user_data\data\binance ...
freqtrade.configuration.configuration - INFO - Parameter --cache=day detected ...
freqtrade.configuration.check_exchange - INFO - Checking exchange...
freqtrade.configuration.check_exchange - INFO - Exchange "binance" is officially supported by the Freqtrade development team.
freqtrade.configuration.configuration - INFO - Using pairlist from configuration.
freqtrade.configuration.config_validation - INFO - Validating configuration ...
freqtrade.commands.optimize_commands - INFO - Starting freqtrade in Backtesting mode
freqtrade.exchange.exchange - INFO - Instance is running with dry_run enabled
freqtrade.exchange.exchange - INFO - Using CCXT 1.93.3
freqtrade.exchange.exchange - INFO - Applying additional ccxt config: {'enableRateLimit': True}
freqtrade.exchange.exchange - INFO - Applying additional ccxt config: {'enableRateLimit': True, 'rateLimit': 200}
freqtrade.exchange.exchange - INFO - Using Exchange "Binance"
freqtrade.resolvers.exchange_resolver - INFO - Using resolved exchange 'Binance'...
freqtrade.resolvers.iresolver - INFO - Using resolved strategy SampleStrategy from freq_research\user_data\strategies\sample_strategy.py'...
freqtrade.strategy.hyper - INFO - Found no parameter file.
freqtrade.resolvers.strategy_resolver - INFO - Override strategy 'stake_currency' with value in config file: USDT.
freqtrade.resolvers.strategy_resolver - INFO - Override strategy 'stake_amount' with value in config file: 100.
freqtrade.resolvers.strategy_resolver - INFO - Override strategy 'unfilledtimeout' with value in config file: {'entry': 10, 'exit': 10, unit': 'minutes'}.
freqtrade.resolvers.strategy_resolver - INFO - Strategy using minimal_roi: {'60': 0.01, '30': 0.02, '0': 0.04}
freqtrade.resolvers.strategy_resolver - INFO - Strategy using timeframe: 5m
freqtrade.resolvers.strategy_resolver - INFO - Strategy using stoploss: -0.1
freqtrade.resolvers.strategy_resolver - INFO - Strategy using trailing_stop: False
freqtrade.resolvers.strategy_resolver - INFO - Strategy using trailing_stop_positive_offset: 0.0
freqtrade.resolvers.strategy_resolver - INFO - Strategy using trailing_only_offset_is_reached: False
freqtrade.resolvers.strategy_resolver - INFO - Strategy using use_custom_stoploss: False
freqtrade.resolvers.strategy_resolver - INFO - Strategy using process_only_new_candles: True
freqtrade.resolvers.strategy_resolver - INFO - Strategy using order_types: {'entry': 'limit', 'exit': 'limit', 'stoploss': 'market', lse}
freqtrade.resolvers.strategy_resolver - INFO - Strategy using order_time_in_force: {'entry': 'GTC', 'exit': 'GTC'}
freqtrade.resolvers.strategy_resolver - INFO - Strategy using stake_currency: USDT
freqtrade.resolvers.strategy_resolver - INFO - Strategy using stake_amount: 100
freqtrade.resolvers.strategy_resolver - INFO - Strategy using protections: []
freqtrade.resolvers.strategy_resolver - INFO - Strategy using startup_candle_count: 30
freqtrade.resolvers.strategy_resolver - INFO - Strategy using unfilledtimeout: {'entry': 10, 'exit': 10, 'exit_timeout_count': 0, 'unit': 
freqtrade.resolvers.strategy_resolver - INFO - Strategy using use_exit_signal: True
freqtrade.resolvers.strategy_resolver - INFO - Strategy using exit_profit_only: False
freqtrade.resolvers.strategy_resolver - INFO - Strategy using ignore_roi_if_entry_signal: False
freqtrade.resolvers.strategy_resolver - INFO - Strategy using exit_profit_offset: 0.0
freqtrade.resolvers.strategy_resolver - INFO - Strategy using disable_dataframe_checks: False
freqtrade.resolvers.strategy_resolver - INFO - Strategy using ignore_buying_expired_candle_after: 0
freqtrade.resolvers.strategy_resolver - INFO - Strategy using position_adjustment_enable: False
freqtrade.resolvers.strategy_resolver - INFO - Strategy using max_entry_position_adjustment: -1
freqtrade.configuration.config_validation - INFO - Validating configuration ...
freqtrade.resolvers.iresolver - INFO - Using resolved pairlist StaticPairList from freqtrade\freqtrade\plugins\pairlist\StaticPairList.py'...
freqtrade.data.history.history_utils - INFO - Using indicator startup period: 30 ...
freqtrade.optimize.backtesting - INFO - Loading data from 2022-09-03 00:00:00 up to 2022-09-12 03:25:00 (9 days).
freqtrade.configuration.timerange - WARNING - Moving start-date by 30 candles to account for startup time.
freqtrade.optimize.backtesting - INFO - Dataload complete. Calculating indicators
freqtrade.optimize.backtesting - WARNING - Backtest result caching disabled due to use of open-ended timerange.
freqtrade.optimize.backtesting - INFO - Running backtesting for Strategy SampleStrategy
freqtrade.strategy.hyper - INFO - No params for buy found, using default values.
freqtrade.strategy.hyper - INFO - Strategy Parameter(default): buy_rsi = 30
freqtrade.strategy.hyper - INFO - Strategy Parameter(default): exit_short_rsi = 30
freqtrade.strategy.hyper - INFO - No params for sell found, using default values.
freqtrade.strategy.hyper - INFO - Strategy Parameter(default): sell_rsi = 70
freqtrade.strategy.hyper - INFO - Strategy Parameter(default): short_rsi = 70
freqtrade.strategy.hyper - INFO - No params for protection found, using default values.
freqtrade.optimize.backtesting - INFO - Backtesting with data from 2022-09-03 02:30:00 up to 2022-09-12 03:25:00 (9 days).
freqtrade.misc - INFO - dumping json to "user_data\backtest_results\backtest-result-2022-09-18_11-43-24.meta.json"
freqtrade.misc - INFO - dumping json to "user_data\backtest_results\backtest-result-2022-09-18_11-43-24.json"
freqtrade.misc - INFO - dumping json to "user_data\backtest_results\.last_result.json"
Result for strategy SampleStrategy