
## 止损

止损配置
```python   
'emergency_exit': 'market',                 # 止损失败时使用
'stoploss_on_exchange': True                # 是否使用交易所止损
'stoploss_on_exchange_interval': 60,        # 止损检查间隔秒
'stoploss_on_exchange_limit_ratio': 0.99    # 触发止损后的限价
```

止损类型
1. 静态止损
2. 追踪止损
3. 触发追踪止损
4. 触发静态止损
5. 自定义止损功能

#### 静态止损
```python
stoploss = -0.10                    # 止损比例
```
止损-10%, 以100美元购买, 跌破90时触发止损

#### 追踪止损
```python
stoploss = -0.10                    # 止损比例
trailing_stop = True                # 是否启用追踪止损
```
止损一直保持在最高价的-10%位置

#### 触发追踪止损
```python
stoploss = -0.10                        # 止损比例
trailing_stop = True                    # 启用追踪止损(必须为True)
trailing_stop_positive = 0.02           # 触发的追踪止损(将会转化为-0.02)
trailing_stop_positive_offset = 0.03    # 触发追踪止损的偏移比例
```
以100美元购买, 若价格上涨超过100*(1+0.03)=103, 止损将变为追踪止损, 止损比例为-0.02

#### 触发静态止损
```python
stoploss = -0.10
trailing_stop = True
trailing_stop_positive = 0.02
trailing_stop_positive_offset = 0.03
trailing_only_offset_is_reached = True
```
以100美元购买, 若价格上涨至100*(1+0.03)=103, 止损将变为静态止损, 止损价=103*(1-0.02)

**注意事项**
确保`trailing_stop_positive_offset`值低于最小ROI, 否则将先应用最小ROI并出售交易

#### 最低投资回报率
```python
"minimal_roi": {
    "40": 0.0,    # 40分钟后如果收益不为负则平仓
    "30": 0.01,   # 30分钟后如果有1%的利润就平仓
    "20": 0.02,   # 20分钟后如果有2%的利润就平仓
    "0":  0.04    # 如果有4%的利润立刻平仓
}
```
1. 设置以分钟为单位
2. 默认设置`{"0": 10}`
3. 设置`{"n": -1}`, 则在n分钟后会强制退出

