---
title: backtesting.py
category: optimize
order: 1
---

## Backtesting
回测对象

- self.config: 配置
- self.results: 
- self.trade_id_counter: 
- self.order_id_counter: 
- 设置回测模式
- self.run_ids: 
- self.strategylist: 策略对象列表
- self.all_results: 
- self.processed_dfs: 
- self._exchange_name: 交易所名称
- self.exchange: 交易所对象
- self.dataprovider: 数据管理器
- 设置多策略/单策略回测
- self.timeframe: 设置回测时间周期
- self.timeframe_min: 时间周期对应的分钟数据
- 时间周期细节校验: self.init_backtest_detail
- self.pairlists: 货币对管理器
- 判断货币对相关设置
- 将货币对管理器添加到数据管理器中
- 刷新货币对管理器
- self.fee: 交易所手续费
- self.precision_mode: 交易所货币精度模式
- self.timerange: 回测时间区间
- self.required_startup: 回测起始要求的K线数据量
- 检查要求的数据量
- 设置AI模型要求的数据量
- 将最终计算的起始数据量添加到配置中
- self.trading_mode: 交易模式
- self._can_short: 是否可做空
- self._position_stacking: 是否允许多次购买同一货币对
- self.enable_protections: 是否启用仓位保护
- 处理binance交易所配置
- 初始化回测: self.init_backtest

#### init_backtest
- 回测准备:self.prepare_backtest
- self.wallets: 钱包对象
- self.progress: 回测进程管理器
- self.abort: 回测终止标志(适用于交互式回测场景)

#### prepare_backtest
- PairLocks.use_db = False
- PairLocks.timeframe = self.config['timeframe']
- Trade.use_db = False
- PairLocks.reset_locks()
- Trade.reset_trades()
- self.rejected_trades = 0
- self.timedout_entry_orders: 
- self.timedout_exit_orders: 
- self.canceled_trade_entries: 
- self.canceled_entry_orders: 
- self.replaced_entry_orders: 
- 清空数据管理器缓存
- 设置仓位保护

#### start
- 加载回测数据: self.load_bt_data
- 如有必要, 加载周期更短的数据: self.load_bt_data_detail
- 尝试加载之前的回测结果: self.load_prior_backtest
- 逐个回测策略: self.backtest_one_strategy
- 汇总所有回测结果: generate_backtest_stats
- 保存回测结果: 
    - store_backtest_stats
    - store_backtest_signal_candles
- 展示回测结果: show_backtest_results

#### load_bt_data
- 通过 history 加载历史数据: history.load_data
- 获取数据起止日期
- 根据"self.required_startup"调整回测起始时间

#### load_bt_data_detail
- 如果配置了"timeframe_detail", 需要加载时间周期更短的数据
- 如果回测的是期货品种, 也需要加载额外数据

#### backtest_one_strategy
回测一个策略
- 策略初始化: self._set_strategy
    - 回调**ft_bot_start**
    - 回调**bot_start(自定义)**
    - 回调**bot_loop_start(自定义)**
- 更新配置: max_open_trades
- self.strategy.advise_all_indicators
    - 回调**advise_all_indicators**: 遍历所有品种数据
    - 回调**advise_indicators**: 添加其他数据
    - 回调**populate_indicators(自定义)**: 计算自定义技术指标
- trim_dataframes: 根据起始时间对数据做修剪
- 获取裁剪后数据的起始和结束时间
- 开始回测主逻辑: self.backtest
    - 回调**advise_entry**
    - 回调**populate_entry_trend(自定义)**: 添加"enter_long/buy"列
    - 回调**populate_exit_trend(自定义)**: 添加"exit_long/sell"列
    - 执行: self.backtest_loop
        - 回调**ft_check_timed_out**
        - 回调**check_entry_timeout(自定义)**
        - 回调**check_exit_timeout(自定义)**
        - 回调**adjust_entry_price(自定义)**
        - 回调**custom_entry_price(自定义)**
        - 回调**custom_stake_amount(自定义)**
        - 回调**confirm_trade_entry(自定义)**
        - 回调**adjust_trade_position(自定义)**
        - 回调**should_exit**
        - 回调**custom_stoploss(自定义)**
        - 回调**custom_exit(自定义)**
        - 回调**min_roi_reached_entry**
        - 回调**custom_exit_price(自定义)**
        - 回调**confirm_trade_exit(自定义)**
- 将所有回测结果保存到: self.all_results
- 检查是否需要导出信号

#### _set_strategy
- self.strategy: 策略对象
- strategy.dp: 策略数据管理器
- strategy.wallets: 策略钱包
- self.strategy.order_types['stoploss_on_exchange']: 参数设置
- self.strategy.ft_bot_start: 回调bot_start
- self.strategy.bot_loop_start: 回调bot_loop_start

#### self.backtest
- 初始化头寸保护: self.prepare_backtest
- 更新钱包: self.wallets.update
- 处理标准化数据(以列表形式返回的标签数据): self._get_ohlcv_as_lists
- 计算起始回测时间: current_time
- 遍历数据
- 校验数据: self.validate_row
- 检查是否有入场/离场信号: self.check_for_trade_entry
- 每次循环对开平仓数据做优化: backtest_loop
    - 回调: should_exit
    - 回调: min_roi_reached_entry
    - 回调: custom_exit_price
    - 回调: confirm_trade_exit
- 处理开平仓信号并记录数据: handle_left_open
- 将开平仓数据转化为DataFrame格式: trade_list_to_dataframe
- 返回所有数据处理结果

#### _get_ohlcv_as_lists

- 删除标签列: ['enter_long', 'exit_long', 'enter_short', 'exit_short', 'enter_tag', 'exit_tag']
- 回调: self.strategy.advise_entry
    - 回调: populate_entry_trend, 添加"enter_long/enter_shrot"列
- 回调: self.strategy.advise_exit
    - 回调: populate_exit_trend, 添加"exit_long/exit_short"列
- trim_dataframes: 根据起始时间对数据做修剪
- 将处理的数据缓存到数据管理器: self.dataprovider._set_cached_df
- 将标签列中的None用0填充(除了'enter_tag'和'exit_tag'), 并shift(1)
- 如果dataframe缺少标签列, 添加该并用0填充('enter_tag'和'exit_tag'用None填充)
- 删除第一行数据, 因为经过shift(1)后, 第一行数据有nan
- 最后仅保留标签列数据, 并以列表形式返回

#### backtest_loop
- self.manage_open_orders
    - self.check_order_cancel
        - self.strategy.ft_check_timed_out
    - self.check_order_replace
        - self.strategy.adjust_entry_price
        - self._enter_trade
            - self.get_valid_price_and_stake
                - self.strategy.custom_entry_price
                - self.strategy.custom_stake_amount
            - self.strategy.confirm_trade_entry
- self._check_trade_exit
    - self._get_adjust_trade_entry_for_candle
        - self.strategy.adjust_trade_position
    - self.strategy.should_exit
    - self._get_exit_for_signal
        - self._get_exit_for_signal
        - self._get_close_rate
            - self._get_close_rate_for_stoploss
            - self._get_close_rate_for_roi
                - self.strategy.min_roi_reached_entry
        - self.strategy.custom_exit_price
        - self.strategy.confirm_trade_exit

