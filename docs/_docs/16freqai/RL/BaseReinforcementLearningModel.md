---
title: BaseReinforcementLearningModel.py
category: freqai/base_models
order: 1
---

## BaseReinforcementLearningModel

#### train

模型训练步骤

1. 获取当前训练批次数据中的特征列和标签列数据: dk.filter_features 
2. 利用sklearn中的`train_test_split`方法构造训练集和测试集数据: dk.make_train_test_datasets
3. 利用scipy计算训练集中的均值和标准差: dk.fit_labels
4. 将以"%-raw_"开头的原始价格数据添加到数据集中
5. 通过训练集数据将训练集和测试集数据归一化: dk.normalize_data
6. 最后数据处理(删除异常值添加噪音等): self.data_cleaning_train
7. 创建模型训练环境: self.set_train_and_eval_environments
7. 模型训练: self.fit

#### predict

模型预测步骤

1. 获取特征标签: dk.find_features
2. 获取验证集特征列: dk.filter_features
3. 根据训练集数据归一化验证集数据:  dk.normalize_data_from_metadata
4. 最后的数据处理: self.data_cleaning_predict
5. 模型预测: self.rl_model_predict
6. 预测数据填充空值: pred_df.fillna

#### build_ohlc_price_dataframes

将以"%-raw_"开头的原始价格数据添加到数据集中

1. 分别获取训练集和测试集数据: train_df, test_df
2. 获取时间周期: tf
3. 修改训练集中原始价格数据列名("%-raw_"): prices_train
4. 修改测试集中原始价格数据列名("%-raw_"): prices_test

#### set_train_and_eval_environments

设置训练环境

1. 分别获取训练集和测试集特征: train_df, test_df
2. 创建环境参数字典: self.pack_env_dict
3. 获取训练集环境(可重写self.MyRLEnv): self.train_env()
4. 使用SB3设置测试集环境: self.eval_env
5. 使用SB3设置回调函数: self.eval_callback
6. 获取动作: self.train_env.get_actions
7. 获取TensorboardCallback: self.tensorboard_callback

#### MyRLEnv

强化学习环境, 默认继承"Base5ActionRLEnv", 可重写`calculate_reward`

#### rl_model_predict

强化学习模型预测

1. 实时运行和回测有区别
2. 通过窗口滑动预测数据
