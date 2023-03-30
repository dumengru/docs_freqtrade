---
title: LightGBMRegressor.py
category: freqai/predection_models
order: 1
---

## LightGBMRegressor

#### fit

训练模型具体步骤

1. 获取训练集数据和权重: eval_set, eval_weights
2. 获取训练特征, 标签和权重因子: x, y, train_weights
3. 查看是否有初始模型模型: self.get_init_model
4. 根据配置"model_training_parameters"构造LGBMRegressor模型
5. 模型训练: model.fit
