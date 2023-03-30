---
title: callback_backtesting
category: stepbystep
order: 3
---

策略描述

dataframe['minute'] % 17 == 0; 做多
dataframe['minute'] % 19 == 0; 做空
dataframe['minute'] % 23 == 0; 多头平仓
dataframe['minute'] % 29 == 0; 空头平仓

策略回调

回调: bot_start
回调: bot_loop_start
回调: populate_indicators
回调: populate_entry_trend
回调: populate_exit_trend


时间: 2022-01-01 02:17:00+00:00; 回调: custom_entry_price; return open: 46888.96
时间: 2022-01-01 02:17:00+00:00; 回调: leverage; return 1.5
时间: 2022-01-01 02:17:00+00:00; 回调: custom_stake_amount; return 50.0
时间: 2022-01-01 02:17:00+00:00; 回调: confirm_trade_entry; return True
时间: 2022-01-01 02:17:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:17:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:18:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:18:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:19:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:19:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:20:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:20:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:21:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:21:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:22:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:22:00+00:00; 回调: custom_exit_price; return close: 46891.17
时间: 2022-01-01 02:22:00+00:00; 回调: confirm_trade_exit; return True
时间: 2022-01-01 02:28:00+00:00; 回调: custom_entry_price; return open: 46829.28
时间: 2022-01-01 02:28:00+00:00; 回调: leverage; return 1.5
时间: 2022-01-01 02:28:00+00:00; 回调: custom_stake_amount; return 50.0
时间: 2022-01-01 02:28:00+00:00; 回调: confirm_trade_entry; return True
时间: 2022-01-01 02:29:00+00:00; 回调: check_entry_timeout; return True
时间: 2022-01-01 02:29:00+00:00; 回调: adjust_entry_price; return current_order_rate: 46829.3
时间: 2022-01-01 02:29:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:29:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:30:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:30:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:31:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:31:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:32:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:32:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:33:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:33:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:34:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:34:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:35:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:35:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:36:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:36:00+00:00; 回调: custom_exit_price; return close: 46784.01
时间: 2022-01-01 02:36:00+00:00; 回调: confirm_trade_exit; return True
时间: 2022-01-01 02:37:00+00:00; 回调: check_exit_timeout; return True
时间: 2022-01-01 02:37:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:37:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:38:00+00:00; 回调: check_exit_timeout; return True
时间: 2022-01-01 02:38:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:38:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:39:00+00:00; 回调: check_exit_timeout; return True
时间: 2022-01-01 02:39:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:39:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:40:00+00:00; 回调: check_exit_timeout; return True
时间: 2022-01-01 02:40:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:40:00+00:00; 回调: custom_exit
时间: 2022-01-01 02:41:00+00:00; 回调: check_exit_timeout; return True
时间: 2022-01-01 02:41:00+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-01-01 02:41:00+00:00; 回调: custom_exit_price; return close: 46757.31
时间: 2022-01-01 02:41:00+00:00; 回调: confirm_trade_exit; return True