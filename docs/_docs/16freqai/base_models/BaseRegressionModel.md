---
title: BaseRegressionModel.py
category: freqai/base_models
order: 1
---

## BaseRegressionModel

#### train

模型训练步骤

1. 获取当前训练批次数据中的特征列和标签列数据: dk.filter_features 
2. 获取起始日期和结束日期: start_date, end_date
3. 利用sklearn中的`train_test_split`方法构造训练集和测试集数据: dk.make_train_test_datasets
4. 利用scipy计算训练集中的均值和标准差: dk.fit_labels
5. 通过训练集数据将训练集和测试集数据归一化: dk.normalize_data
6. 最后数据处理(删除异常值添加噪音等): self.data_cleaning_train
7. 模型训练: self.fit

#### predict

模型预测步骤

1. 获取特征标签: dk.find_features
2. 获取验证集特征列: dk.filter_features
3. 根据训练集数据归一化验证集数据:  dk.normalize_data_from_metadata
4. 最后的数据处理: self.data_cleaning_predict
5. 模型预测: self.model.predict
6. 反归一预测数据: dk.denormalize_labels_from_metadata