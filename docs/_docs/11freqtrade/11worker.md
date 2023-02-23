---
title: worker.py
category: freqtrade
order: 11
---

## Worker

#### __init__初始化
1. 获取配置: self._config
2. self._init(False)

#### _init
1. 初始化bot: self.freqtrade = FreqtradeBot(self._config)
2. 获取配制: self._throttle_secs(进程循环周期, 默认5s)
3. 获取: self._heartbeat_interval(心跳周期, 默认60s)
4. 创建消息对象: self._sd_notify

#### run()
1. 设置初始状态为空: state = None
2. 死循环执行: self._worker(old_state=state)
3. 根据状态判断是否重新加载配置: self._reconfigure()

#### _reconfigure()
- 重新加载配置

#### _worker()
- 主程序
1. 获取bot状态: state = self.freqtrade.state
2. 状态改变
    - State.RUNNING
        - 启动bot: self.freqtrade.startup()
    - State.STOPPED
        - 检查交易: self.freqtrade.check_for_open_trades()
3. State.STOPPED
    - 回调self._process_stopped
4. State.RUNNING
    - 回调self._process_running
5. 输出self._heartbeat_interval信息

#### _process_stopped()
1. 停止bot: self.freqtrade.process_stopped()

#### _process_running()
1. 处理交易: self.freqtrade.process()
2. 抛出异常则停止程序