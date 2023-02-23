---
title: dry_run
category: stepbystep
order: 4
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
时间: 2022-09-30 06:26:42.426289+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:26:42.426289+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:26:47.226821+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:26:47.226821+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:26:52.229470+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:26:52.229470+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:26:57.238682+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:26:57.238682+00:00; 回调: custom_exit
回调: bot_loop_start
回调: populate_indicators
回调: populate_entry_trend
回调: populate_exit_trend
时间: 2022-09-30 06:27:02.720622+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:27:02.720622+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:27:07.267761+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:27:07.267761+00:00; 回调: custom_exit
时间: 2022-09-30 06:41:04.265369+00:00; 回调: custom_entry_price; return open: 19420.1
时间: 2022-09-30 06:41:04.266370+00:00; 回调: leverage; return 5
时间: 2022-09-30 06:41:04.269367+00:00; 回调: custom_stake_amount; return 50.0
debot - INFO - Long signal found: about create a new trade for BTC/USDT with stake_amount: 50.0 ...
时间: 2022-09-30 06:41:04.271371+00:00; 回调: confirm_trade_entry; return True
debot - INFO - Order #dry_run_buy_1664520064.272371 was created for BTC/USDT and status is open.
 - INFO - Wallets synced.
_manager - INFO - Sending rpc message: {'trade_id': 3, 'type': entry, 'buy_tag': 'enter_long', 'enter_tag': 'enter_long', 'exchange': 'Binance', 'pair': 'BTC/USDT', t': 19420.1, 'open_rate': 19420.1, 'order_type': 'limit', 'stake_amount': 50.0, 'stake_currency': 'USDT', 'fiat_currency': 'USD', 'amount': 0.012, 'open_date': 0374), 'current_rate': 19425.8, 'sub_trade': False}
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:08.669466+00:00; 回调: check_entry_timeout; return False
debot - INFO - No currency pair in active pair whitelist, but checking to exit open trades.
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:13.679760+00:00; 回调: check_entry_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:18.693480+00:00; 回调: check_entry_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:23.706486+00:00; 回调: check_entry_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:28.709923+00:00; 回调: check_entry_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:33.723829+00:00; 回调: check_entry_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:38.723492+00:00; 回调: check_entry_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:43.736755+00:00; 回调: check_entry_timeout; return False
- INFO - Bot heartbeat. PID=5336, version='2022.10.dev-80d0e66b4', state='RUNNING'
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:48.753581+00:00; 回调: check_entry_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:53.766745+00:00; 回调: check_entry_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:41:58.774387+00:00; 回调: check_entry_timeout; return False
回调: bot_loop_start
回调: populate_indicators
回调: populate_entry_trend
回调: populate_exit_trend
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
debot - INFO - Fee for Trade Trade(id=3, pair=BTC/USDT, amount=0.01287326, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 06:41:04) [buy]
ence.trade_model - INFO - Updating trade (id=3) ...
ence.trade_model - INFO - LIMIT_BUY has been fulfilled for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, 
 - INFO - Wallets synced.
_manager - INFO - Sending rpc message: {'trade_id': 3, 'type': entry_fill, 'buy_tag': 'enter_long', 'enter_tag': 'enter_long', 'exchange': 'Binance', 'pair': 'BTC/, 'limit': 19420.1, 'open_rate': 19420.1, 'order_type': None, 'stake_amount': 46.60824, 'stake_currency': 'USDT', 'fiat_currency': 'USD', 'amount': 0.012, 'open_date': 0374), 'current_rate': 19425.8, 'sub_trade': False}
egram - INFO - Notification 'entry_fill' not sent.
时间: 2022-09-30 06:42:04.236399+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:04.236399+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:42:08.782261+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:08.782261+00:00; 回调: custom_exit
debot - INFO - No currency pair in active pair whitelist, but checking to exit open trades.
回调: bot_loop_start
时间: 2022-09-30 06:42:13.784262+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:13.784262+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:42:18.799082+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:18.799082+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:42:23.808424+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:23.808424+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:42:28.820168+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:28.820168+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:42:33.821868+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:33.821868+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:42:38.840236+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:38.840236+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:42:43.848293+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:43.848293+00:00; 回调: custom_exit
- INFO - Bot heartbeat. PID=5336, version='2022.10.dev-80d0e66b4', state='RUNNING'
回调: bot_loop_start
时间: 2022-09-30 06:42:48.864193+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:48.864193+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:42:53.873101+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:53.873101+00:00; 回调: custom_exit
回调: bot_loop_start
时间: 2022-09-30 06:42:58.877607+00:00; 回调: custom_stoploss; return -0.5
时间: 2022-09-30 06:42:58.877607+00:00; 回调: custom_exit
回调: bot_loop_start
回调: populate_indicators
回调: populate_entry_trend
回调: populate_exit_trend
时间: 2022-09-30 06:43:04.448754+00:00; 回调: custom_stoploss; return -0.5
debot - INFO - Exit for BTC/USDT detected. Reason: exit_signal Tag: exit_long
时间: 2022-09-30 06:43:05.045749+00:00; 回调: custom_exit_price; return close: 19414.4
 - INFO - Wallets synced.
时间: 2022-09-30 06:43:05.050754+00:00; 回调: confirm_trade_exit; return True
_manager - INFO - Sending rpc message: {'type': exit, 'trade_id': 3, 'exchange': 'Binance', 'pair': 'BTC/USDT', 'leverage': 5.0, 'direction': 'Long', 'gain': 'loss', rder_type': 'limit', 'amount': 0.012, 'open_rate': 19420.1, 'close_rate': 19414.4, 'current_rate': 19410.8, 'profit_amount': -0.1616028, 'profit_ratio': -0.00346656, r_long', 'sell_reason': 'exit_long', 'exit_reason': 'exit_long', 'open_date': datetime.datetime(2022, 9, 30, 6, 41, 4, 380374), 'close_date': datetime.datetime(2022, 6.60824, 'stake_currency': 'USDT', 'fiat_currency': 'USD', 'sub_trade': False, 'cumulative_profit': 0.0}
 - INFO - Wallets synced.
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:08.900082+00:00; 回调: check_exit_timeout; return False
debot - INFO - No currency pair in active pair whitelist, but checking to exit open trades.
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:13.904547+00:00; 回调: check_exit_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:18.915874+00:00; 回调: check_exit_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:23.922383+00:00; 回调: check_exit_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:28.931528+00:00; 回调: check_exit_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:33.925946+00:00; 回调: check_exit_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:38.936613+00:00; 回调: check_exit_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:43.958261+00:00; 回调: check_exit_timeout; return False
- INFO - Bot heartbeat. PID=5336, version='2022.10.dev-80d0e66b4', state='RUNNING'
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:48.951595+00:00; 回调: check_exit_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:53.954065+00:00; 回调: check_exit_timeout; return False
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:43:58.966538+00:00; 回调: check_exit_timeout; return False
回调: bot_loop_start
回调: populate_indicators
回调: populate_entry_trend
回调: populate_exit_trend
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:04.384421+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:04.472426+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:08.987828+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:09.087254+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
debot - INFO - No currency pair in active pair whitelist, but checking to exit open trades.
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:14.000154+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:14.097428+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:18.996215+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:19.084747+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:24.005315+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:24.089324+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:29.015129+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:29.099458+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:34.033519+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:34.115468+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:39.050365+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:39.146635+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:44.050947+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:44.149692+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
- INFO - Bot heartbeat. PID=5336, version='2022.10.dev-80d0e66b4', state='RUNNING'
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:49.071076+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:49.152873+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:54.071449+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:54.171470+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:44:59.075670+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:44:59.171049+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
回调: bot_loop_start
回调: populate_indicators
回调: populate_entry_trend
回调: populate_exit_trend
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
时间: 2022-09-30 06:45:04.579790+00:00; 回调: check_exit_timeout; return False
时间: 2022-09-30 06:45:04.678793+00:00; 回调: adjust_entry_price; return current_order_rate: 19414.4
 - INFO - Wallets synced.
回调: bot_loop_start
debot - INFO - Found open order for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 
debot - INFO - Fee for Trade Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=2022-09-30 06:41:04) 
ence.trade_model - INFO - Updating trade (id=3) ...
ence.trade_model - INFO - LIMIT_SELL has been fulfilled for Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, 
ence.trade_model - INFO - Marking Trade(id=3, pair=BTC/USDT, amount=0.01200000, is_short=False, leverage=5.0, open_rate=19420.10000000, open_since=closed) as closed as ders for it.
 - INFO - Wallets synced.
_manager - INFO - Sending rpc message: {'type': exit_fill, 'trade_id': 3, 'exchange': 'Binance', 'pair': 'BTC/USDT', 'leverage': 5.0, 'direction': 'Long', 'gain': 14.4, 'order_type': '', 'amount': 0.012, 'open_rate': 19420.1, 'close_rate': 19414.4, 'current_rate': None, 'profit_amount': -0.1616028, 'profit_ratio': -0.00346656, r_long', 'sell_reason': 'exit_long', 'exit_reason': 'exit_long', 'open_date': datetime.datetime(2022, 9, 30, 6, 41, 4, 380374), 'close_date': datetime.datetime(2022, .60824, 'stake_currency': 'USDT', 'fiat_currency': 'USD', 'sub_trade': False, 'cumulative_profit': -0.1616028}
egram - INFO - Notification 'exit_fill' not sent.
 - INFO - Wallets synced.
