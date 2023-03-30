---
title: ReinforcementLearner.py
category: freqai/predection_models
order: 1
---

## ReinforcementLearner

#### fit
训练模型具体步骤

1. 获取训练集数据特征: train_df
2. 计算迭代总次数: total_timesteps = 配置`train_cycles`*len(train_df)
3. 获取神经网络参数: policy_kwargs
4. 如果配置`continual_learning=False`, 重新构造SB3模型, 否则使用原有模型
5. 设置模型环境: model.set_env
6. 模型学习: model.learn
7. 如果发现最优模型模型立即返回
