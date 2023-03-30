---
title: trade_model.py
category: persistence
order: 1
---

## Order(_DECL_BASE)

#### 关键列
1. 主键: id
2. 外键: ft_trade_id
3. 联合主键: ft_pair， order_id
4. 索引: ft_trade_id,  ft_is_open, order_id

### 属性
1. order_date_utc: 将 order_data 改为UTC时区
2. order_filled_utc: 将 order_filled_utc 改为UTC时区
3. safe_filled: 获取filled或amount
4. safe_fee_base: 获取ft_fee_base
5. safe_amount_after_fee: 计算 safe_filled - safe_fee_base

#### 方法
1. update_from_ccxt_object: 从ccxt响应更新order数据
2. to_ccxt_object: 返回字典类型order数据
3. to_json: 返回字典类型order数据
4. close_bt_order: 关闭order后更新数据
5. update_orders: 更新活跃订单数据
6. parse_from_ccxt_object: 从ccxt获取订单数据并更新
7. get_open_orders: 获取全部活跃订单
8. order_by_id: 通过id查询订单

## LocalTrade()
交易对象基类

#### 类属性
use_db: 
trades: LocalTrade对象列表
trades_open: 活跃LocalTrade对象列表
total_profit: 
realized_profit: 

id: 
orders: Order对象列表

exchange: 
pair: 
base_currency: 
stake_currency: 
is_open: 

fee_open: 
fee_open_cost: 
fee_open_currency: 
fee_close: 
fee_close_cost: 
fee_close_currency: 

open_rate: 开仓价
open_rate_requested: 
open_trade_value: 
close_rate: 
close_rate_requested: 
close_trade_value: 

close_profit: 
close_profit_abs:
amount:  
stake_amount: 
open_date: 
open_order_id: 

stop_loss: 绝对止损
stop_loss_pct: 百分比止损
initial_stop_loss: 
initial_stop_loss_pct: 
stoploss_order_id: 止损订单id
stoploss_last_update: 止损订单日期

max_rate: 最高成交价
min_rate: 最低成交价
exit_reason: 退出原因
exit_order_status: 
strategy: 
enter_tag: 
timeframe: 

trading_mode: 交易模式
amount_precision: 
price_precision: 
precision_mode: 
contract_size: 

liquidation_price: 
is_short: 
leverage: 

interest_rate: 保证金比率
funding_fees: 期货属性

#### 其他属性
stoploss_or_liquidation: 成交价
buy_tag: 
has_no_leverage: 是否有杠杆
borrowed: 借贷额
date_last_filled_utc: 最新成交单utc时间
open_date_utc: 开仓utc时间
close_date_utc: 平仓utc时间
enter_side: 开仓方向
exit_side: 平仓方向
trade_direction: 交易方向
safe_base_currency: 基础货币
safe_quote_currency: 报价货币

#### 方法
to_json: 返回字典格式的交易数据
reset_trades: 回测是重置交易数据
adjust_min_max_rates: 调整max_rate和min_rate
set_liquidation_price: 
__set_stop_loss: 设置止损
adjust_stop_loss: 调整止损
update_trade: 更新交易数据
close: 平仓
update_fee: 更新资金费率
fee_updated: 判断费率是否已更新
update_order: 更新订单对象
get_exit_order_count: 获取撤单数量
_calc_open_trade_value: 计算开仓价值(包含费用)
recalc_open_trade_value: 从新计算开仓价值
calculate_interest: 计算利息(保证金交易中)
calc_close_trade_value: 计算平仓价值
calc_profit: 计算利润
calc_profit_ratio: 计算利润率
recalc_trade_from_orders: 根据订单重新计算交易数据
select_order_by_order_id: 通过订单id选择订单
select_order: 选择订单
select_filled_orders: 选择已成交订单
select_filled_or_open_orders: 选择已成交或开仓订单
nr_of_successful_entries: 计算成功执行的订单数量
nr_of_successful_exits: 计算成功撤单数量
nr_of_successful_buys: 计算买单数量
nr_of_successful_sells: 计算卖单数量
get_trades_proxy: 查询成交
close_bt_trade: 平仓更新数据
add_bt_trade: 开仓添加数据
get_open_trades: 查询开仓单
get_open_trade_count: 获取开仓单数量
stoploss_reinitialization: 初始化交易止损数据

## class Trade(_DECL_BASE, LocalTrade)

#### 关键列
id: 主键
索引列: is_open, pair, stoploss_order_id

#### 属性


#### 方法
delete: 删除
commit: 提交
rollback: 回滚
get_trades_proxy: 查询交易数据
get_trades: 查询订单
get_open_order_trades: 获取所有开仓交易
get_open_trades_without_assigned_fees: 获取所有没有正确设置费用的开仓交易
get_closed_trades_without_assigned_fees: 获取所有没有正确设置费用的关闭交易
get_total_closed_profit: 获取所有关闭交易的利润
total_open_trades_stakes: 计算全部开仓交易的资金
get_overall_performance: 以列表形式获取所有交易的结果
get_enter_tag_performance: 
get_exit_reason_performance: 
get_mix_tag_performance: 
get_best_pair: 
get_trading_volume: 根据订单获取成交量

