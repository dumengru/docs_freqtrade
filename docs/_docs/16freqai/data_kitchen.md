---
title: data_kitchen.py
category: freqai
order: 3
---

## FreqaiDataKitchen

- self.data: 
- self.data_dictionary: 
- self.config: 全部配置
- self.freqai_config: freqai配置
- self.full_df: 
- self.append_df: 
- self.data_path: 
- self.label_list:
- self.training_features_list: 
- self.model_filename: 
- self.backtesting_results_path: 
- self.backtest_predictions_folder: 
- self.live: 是否在线运行
- self.pair: 货币对

- self.svm_model: svm分类器模型
- self.keras: 是否使用keras
- 保存所有货币对到self.all_pairs: self.set_all_pairs()
- self.backtest_live_models: 
- self.full_path: 模型路径
- self.full_timerange: 根据配置"train_period_days"调整回测起始时间区间
- self.training_timeranges, self.backtesting_timeranges: 将回测数据分割, 按照配置中的"train_period_days"分割训练数据(训练集+测试集), "backtest_period_days"分割验证集数据
- 计算需要使用的cpu数量
- self.train_dates: 
- self.unique_classes: 
- self.unique_class_list: 
- self.backtest_live_models_data: 


#### create_fulltimerange

假设回测区间是"20220101-20230101", 该函数根据配置"train_period_days=15"调整起始时间区间"20211217-20230101"

#### split_timerange

假设回测数据调整后起始时间区间"20211217-20230101", 配置中"train_period_days=15", "backtest_period_days=7".

将回测数据时间区间分割
- tr_training_list_timerange: 从20211217开始, 每隔15天一批数据
- tr_backtesting_list_timerange: 从20211217+15开始, 每隔7天一批数据

#### slice_dataframe

根据时间区间起始位置切割数据, 因为回测时获取的数据仅用时间的结束位置做判断获取数据, 因此这里需要用起始位置做调整

#### find_features

获取数据中的特征列名, 所有以"%"开头的列都是特征列

#### find_labels

获取数据中的标签列名, 所有以"&"开头的列都是标签列

#### filter_features

根据特征列名和标签列名获取数据中的特征列和标签列并返回, 训练数据和验证数据有不同处理, 验证集没有标签列

#### make_train_test_datasets

1. 根据配置中的"data_split_parameters"参数将训练数据分为训练集和测试集利用了
`from sklearn.model_selection import train_test_split`方法

#### set_weights_higher_recent

根据配置中的"freqai.feature_parameters.weight_factor"参数, 为数据构建一个权重序列

#### fit_labels

利用scipy计算训练集中的均值和标准差

#### normalize_data

通过训练集数据将训练集和测试集数据归一化

#### compute_inlier_metric

衡量当前数据点与最近的历史数据点的相似程度

#### principal_component_analysis

PCA降维

#### use_SVM_to_remove_outliers

SVM移除异常值

#### use_DBSCAN_to_remove_outliers

使用DBSCAN聚类识别异常值

#### add_noise_to_training_features

向训练特征中添加噪声防止数据过拟合

#### fit

训练模型

#### save_data

模型保存

#### normalize_data_from_metadata

根据训练集数据归一化验证集数据

#### denormalize_labels_from_metadata

反归一化预测数据

#### get_predictions_to_append

将验证集标签, 预测值和DI值合并

#### append_predictions

将训练数据和验证数据合并

#### save_backtesting_prediction

保存预测结果到本地

#### fill_predictions

合并所有的价格, 特征, 标签和预测数据

#### remove_features_from_df

删除所有的特征标签列