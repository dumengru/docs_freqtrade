---
title: info_dryrun
category: stepbystep
order: 6
---

(env_crypto) D:\DuWork\github_project\freq_research>freqtrade trade --config config.json --strategy SampleStrategy
freqtrade.worker - INFO - Starting worker 2022.9.dev
freqtrade.configuration.load_config - INFO - Using config: config.json ...
freqtrade.loggers - INFO - Verbosity set to 0
freqtrade.configuration.configuration - INFO - Runmode set to dry_run.
freqtrade.configuration.configuration - INFO - Dry run is enabled
freqtrade.configuration.configuration - INFO - Using DB: "sqlite:///tradesv3.dryrun.sqlite"
freqtrade.configuration.configuration - INFO - Using max_open_trades: 3 ...
freqtrade.configuration.configuration - INFO - Using user-data directory: user_data ...
freqtrade.configuration.configuration - INFO - Using data directory: user_data\data\binance ...
freqtrade.configuration.check_exchange - INFO - Checking exchange...
freqtrade.configuration.check_exchange - INFO - Exchange "binance" is officially supported by the Freqtrade development team.
freqtrade.configuration.configuration - INFO - Using pairlist from configuration.
freqtrade.freqtradebot - INFO - Starting freqtrade 2022.9.dev
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
freqtrade.exchange.exchange - INFO - Instance is running with dry_run enabled
freqtrade.exchange.exchange - INFO - Using CCXT 1.93.3
freqtrade.exchange.exchange - INFO - Applying additional ccxt config: {'enableRateLimit': True}
freqtrade.exchange.exchange - INFO - Applying additional ccxt config: {'enableRateLimit': True, 'rateLimit': 200}
freqtrade.exchange.exchange - INFO - Using Exchange "Binance"
freqtrade.resolvers.exchange_resolver - INFO - Using resolved exchange 'Binance'...
freqtrade.wallets - INFO - Wallets synced.
freqtrade.rpc.rpc_manager - INFO - Enabling rpc.api_server
freqtrade.rpc.api_server.webserver - INFO - Starting HTTP Server at 127.0.0.1:8080
freqtrade.rpc.api_server.webserver - INFO - Starting Local Rest Server.
uvicorn.error - INFO - Started server process [15320]
uvicorn.error - INFO - Waiting for application startup.
uvicorn.error - INFO - Application startup complete.
uvicorn.error - INFO - Uvicorn running on http://127.0.0.1:8080 (Press CTRL+C to quit)
freqtrade.resolvers.iresolver - INFO - Using resolved pairlist StaticPairList from freqtrade\freqtrade\plugins\pairlist\StaticPairList.py'...
freqtrade.strategy.hyper - INFO - No params for buy found, using default values.
freqtrade.strategy.hyper - INFO - Strategy Parameter(default): buy_rsi = 30
freqtrade.strategy.hyper - INFO - Strategy Parameter(default): exit_short_rsi = 30
freqtrade.strategy.hyper - INFO - No params for sell found, using default values.
freqtrade.strategy.hyper - INFO - Strategy Parameter(default): sell_rsi = 70
freqtrade.strategy.hyper - INFO - Strategy Parameter(default): short_rsi = 70
freqtrade.strategy.hyper - INFO - No params for protection found, using default values.
freqtrade.plugins.protectionmanager - INFO - No protection Handlers defined.
freqtrade.rpc.rpc_manager - INFO - Sending rpc message: {'type': status, 'status': 'running'}
freqtrade.worker - INFO - Changing state to: RUNNING
freqtrade.rpc.rpc_manager - INFO - Sending rpc message: {'type': warning, 'status': 'Dry run is enabled. All trades are simulated.'}
freqtrade.rpc.rpc_manager - INFO - Sending rpc message: {'type': startup, 'status': "*Exchange:* `binance`\n*Stake per trade:* `100 0': 0.01, '30': 0.02, '0': 0.04}`\n*Stoploss:* `-0.1`\n*Position adjustment:* `Off`\n*Timeframe:* `5m`\n*Strategy:* `SampleStrategy`"}
freqtrade.rpc.rpc_manager - INFO - Sending rpc message: {'type': startup, 'status': "Searching for USDT pairs to buy and sell based on cPairList'}]"}
freqtrade.worker - INFO - Bot heartbeat. PID=15320, version='2022.9.dev', state='RUNNING'
freqtrade.worker - INFO - Bot heartbeat. PID=15320, version='2022.9.dev', state='RUNNING'
freqtrade.commands.trade_commands - INFO - SIGINT received, aborting ...
freqtrade.commands.trade_commands - INFO - worker found ... calling exit
freqtrade.rpc.rpc_manager - INFO - Sending rpc message: {'type': status, 'status': 'process died'}
freqtrade.freqtradebot - INFO - Cleaning up modules ...
freqtrade.rpc.rpc_manager - INFO - Cleaning up rpc modules ...
freqtrade.rpc.rpc_manager - INFO - Cleaning up rpc.apiserver ...
freqtrade.rpc.api_server.webserver - INFO - Stopping API Server
uvicorn.error - INFO - Shutting down
uvicorn.error - INFO - Waiting for application shutdown.
uvicorn.error - INFO - Application shutdown complete.
uvicorn.error - INFO - Finished server process [15320]