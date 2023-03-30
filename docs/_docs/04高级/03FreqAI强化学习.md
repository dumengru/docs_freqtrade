---
title: FreqAI强化学习
category: 高级
order: 3
---

![](../../images/202302281701.png)

AI相关参考文档:

- scikit-learn文档: https://scikit-learn.org/stable/user_guide.html
- Keras文档: https://keras.io/zh/
- PyTorch文档: https://www.pytorchtutorial.com/docs/

- 动手学深度学习: http://zh.d2l.ai/
- 动手学强化学习: https://hrl.boyuai.com/chapter/intro

- stable-baselines3: https://stable-baselines3.readthedocs.io/en/master/index.html
- OpenAI gym: https://www.gymlibrary.dev/content/basic_usage/

## 简介

强化学习涉及两个重要的组成部分: 智能体和训练环境. 在智能体训练期间, 智能体逐根移动历史K线数据, 智能体的动作空间包括(多头入场, 多头离场, 空头入场, 空头离场, 中性), 用户可以通过重写"calculate_reward"方法自定义奖励

FreqAI的强化学习框架建立在stable_baselines3(torch)和OpenAI gym的基础环境类上, 用户只需要将精力集中在"calculate_reward"设计上即可


## 配置

```json
"rl_config": {
    "train_cycles": 25,
    "add_state_info": true,
    "max_trade_duration_candles": 300,
    "max_training_drawdown_pct": 0.02,
    "cpu_count": 8,
    "model_type": "PPO",
    "policy_type": "MlpPolicy",
    "model_reward_parameters": {
        "rr": 1,
        "profit_aim": 0.025
    }
}
```

#### 参数表

- rl_config: 所有freqai强化学习相关配置都在该字典内
- train_cycles: 训练次数
- cpu_count: 使用cpu数量
- max_trade_duration_candles: 一定时间内最大开仓次数
- model_type: stable_baselines3或SBcontrib的模型名称 
- policy_type: 来自stable_baselines3的可用策略类型
- max_training_drawdown_pct: 允许最大回撤
- model_reward_parameters: 编写"calculate_reward"方法需要使用的自定义参数
- add_state_info: 使用启用状态信息(包括交易持续时间, 当前利润, 交易头寸等), 回测不可用, 默认false
- randomize_starting_position: 随机化每个起点避免过拟合, 默认false

- net_arch: 神经网络隐藏层数量, 默认[128, 128]
- continual_learning: 使用最新模型, 默认False
- conv_width: 卷积神经网络张量宽度, 默认2

## 策略编写

假设我们将智能体动作用数字表示: 中性(0), 多头入场(1), 多头离场(2), 空头入场(3), 空头离场(4)

#### 特征工程

FreqAI RL的特征工程和之前的机器学习大部分内容一致(使用"feature_engineering_*"方法构造), 区别有两点:

1. "set_freqai_targets"方法: 监督式学习需要使用标签, 强化学习不需要, 但是必须将该列设为中性值

```python
def set_freqai_targets(self, dataframe, **kwargs):
    dataframe["&-action"] = 0
```

2. "feature_engineering_standard"方法: 必须将原始数据传递给智能体, 以便它可以在训练环境中访问原始OHLCV

```python
def feature_engineering_standard(self, dataframe, **kwargs):
    # The following features are necessary for RL models
    dataframe[f"%-raw_close"] = dataframe["close"]
    dataframe[f"%-raw_open"] = dataframe["open"]
    dataframe[f"%-raw_high"] = dataframe["high"]
    dataframe[f"%-raw_low"] = dataframe["low"]
```

#### 策略逻辑

```python
def populate_entry_trend(self, df: DataFrame, metadata: dict) -> DataFrame:
    # 智能体执行多头入场动作
    enter_long_conditions = [df["do_predict"] == 1, df["&-action"] == 1]
    if enter_long_conditions:
        df.loc[
            reduce(lambda x, y: x & y, enter_long_conditions), ["enter_long", "enter_tag"]
        ] = (1, "long")
    # 智能体执行空头入场动作
    enter_short_conditions = [df["do_predict"] == 1, df["&-action"] == 3]
    if enter_short_conditions:
        df.loc[
            reduce(lambda x, y: x & y, enter_short_conditions), ["enter_short", "enter_tag"]
        ] = (1, "short")
    return df

def populate_exit_trend(self, df: DataFrame, metadata: dict) -> DataFrame:
    # 智能体执行多头离场动作
    exit_long_conditions = [df["do_predict"] == 1, df["&-action"] == 2]
    if exit_long_conditions:
        df.loc[reduce(lambda x, y: x & y, exit_long_conditions), "exit_long"] = 1
    # 智能体执行空头离场动作
    exit_short_conditions = [df["do_predict"] == 1, df["&-action"] == 4]
    if exit_short_conditions:
        df.loc[reduce(lambda x, y: x & y, exit_short_conditions), "exit_short"] = 1

    return df
```

#### 自定义奖励示例

FreqAI 提供三个基础环境: Base3ActionRLEnvironment, Base4ActionEnvironment和Base5ActionEnvironment

Base3ActionEnvironment中的智能体可以选择中立, 做多, 平仓. 仅适用于只做多头的机器人

Base4ActionEnvironment中的智能体可以选择做多, 做空, 中立或退出头寸

Base5ActionEnvironment中的智能体包含Base4所有的操作, 但它将退出分为多头退出和空头退出

```python
from freqtrade.freqai.prediction_models.ReinforcementLearner import ReinforcementLearner
from freqtrade.freqai.RL.Base5ActionRLEnv import Actions, Base5ActionRLEnv, Positions


class MyCoolRLModel(ReinforcementLearner):
    class MyRLEnv(Base5ActionRLEnv):
        """
        Base5ActionRLEnv继承自BaseEnvironment和gym.env, MyRLEnv继承Base5后可重写 calculate_reward 方法
        """
        def calculate_reward(self, action: int) -> float:
            # first, penalize if the action is not valid
            if not self._is_valid(action):
                return -2
            pnl = self.get_unrealized_profit()

            factor = 100

            # you can use feature values from dataframe
            # Assumes the shifted RSI indicator has been generated in the strategy.
            rsi_now = self.raw_features[f"%-rsi-period-10_shift-1_{self.pair}_"
                            f"{self.config['timeframe']}"].iloc[self._current_tick]

            # reward agent for entering trades
            if (action in (Actions.Long_enter.value, Actions.Short_enter.value)
                    and self._position == Positions.Neutral):
                if rsi_now < 40:
                    factor = 40 / rsi_now
                else:
                    factor = 1
                return 25 * factor

            # discourage agent from not entering trades
            if action == Actions.Neutral.value and self._position == Positions.Neutral:
                return -1
            max_trade_duration = self.rl_config.get('max_trade_duration_candles', 300)
            trade_duration = self._current_tick - self._last_trade_tick
            if trade_duration <= max_trade_duration:
                factor *= 1.5
            elif trade_duration > max_trade_duration:
                factor *= 0.5
            # discourage sitting in position
            if self._position in (Positions.Short, Positions.Long) and \
            action == Actions.Neutral.value:
                return -1 * trade_duration / max_trade_duration
            # close long
            if action == Actions.Long_exit.value and self._position == Positions.Long:
                if pnl > self.profit_aim * self.rr:
                    factor *= self.rl_config['model_reward_parameters'].get('win_reward_factor', 2)
                return float(pnl * factor)
            # close short
            if action == Actions.Short_exit.value and self._position == Positions.Short:
                if pnl > self.profit_aim * self.rr:
                    factor *= self.rl_config['model_reward_parameters'].get('win_reward_factor', 2)
                return float(pnl * factor)
            return 0.
```

## 运行

```python
freqtrade trade --freqaimodel ReinforcementLearner --strategy MyRLStrategy --config config.json
```
