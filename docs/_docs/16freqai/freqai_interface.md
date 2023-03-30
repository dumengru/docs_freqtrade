---
title: freqai_interface.py
category: freqai
order: 1
---

## IFreqaiModel

- self.config: 配置
- self.assert_config: 校验freqai配置
- self.freqai_info: 获取freqai的配置
- self.data_split_parameters: 数据集分割参数
- self.model_training_parameters: 模型训练参数
- self.identifier: 模型id
- self.retrain: 是否重复训练
- 设置模型保存路径: self.set_full_path()
- self.save_backtest_models: 是否保存模型
- self.dd: 模型信息管理器FreqaiDataDrawer对象
- self.current_candle: 当前K线的时间
- self.ft_params: 因子特征参数
- self.corr_pairlist: 货币对列表
- self.keras: 是否使用Keras
- self.pair_it: 运行模型的货币对数量
- self.pair_it_train = 0
- self.total_pairs: 货币白名单数量
- self.train_queue: 设置训练货币对队列
- self.inference_time: float = 0
- self.train_time: 
- self.begin_time: 模型启动事件
- self.begin_time_train: float = 0
- self.base_tf_seconds: 策略时间周期(秒)
- self.plot_features: 绘制特征重要性
- self._threads: 线程列表
- self._stop_event: 线程事件
- self.metadata: "global_metadata.json"数据
- self.data_provider: 数据管理器
- self.max_system_threads: 系统最多可使用的线程数量
- self.can_short: 是否可做空
- 将参数保存到"run_params.json"文件

#### _set_train_queue

设置训练货币对队列

1. 获取货币白名单
2. 将货币对保存到队列中
3. 创建一个"best_queue"
4. 将数据管理器中的货币对字典按训练时间戳排序
5. 遍历排序货币对字典 如果第一个货币对在白名单中则添加到"best_queue"
6. 遍历白名单, 如果货币不在"best_queue"则添加
7. 返回"best_queue"

#### start
1. 是否仿真/实盘运行: self.live
2. 将"populate_indicators"中的"metadata"传入模型信息管理器
3. 将策略中的数据管理器赋值给"self.data_provider"
4. 将策略的"can_short"赋值给"self.can_short"
5. 如果是仿真/实盘运行
- 设置启动时间
- 创建货币对模型管理器"self.dk"
- 训练并预测模型"self.start_live", 并返回模型管理器
- 移除数据中的特征列
5. 如果是回测
- 创建货币对模型管理器"self.dk"
- 训练并预测模型"self.start_backtesting", 并返回模型管理器
- 移除数据中的特征列
6. 清理缓存的模型和数据

#### start_live

1. 获取货币对信息
2. 更新历史数据
3. 检查是否重复训练
4. 设置训练的文件路径
5. 如果"self.dd"没有对应的历史数据则加载所有货币对的历史数据
6. 启动线程训练模型
7. 加载模型
8. 根据模型重新计算指标
9. 完成模型预测: build_strategy_return_arrays
10. 返回货币对模型管理器

#### start_scanning

开辟一个线程启动"self._start_scanning"

#### _start_scanning

完成模型训练逻辑

#### start_backtesting

- self.pair_it: 回测货币数量
- train_it: 训练次数
- pair: 当前训练货币对名称
- populate_indicators: 是否计算完特征数据, 防止多批次训练重复计算
- check_features = True
- 遍历FreqaiDataKitchen中的训练数据(训练集和测试集)和验证集时间列表
    1. 计算验证集训练批次: total_trains
    2. 获取训练数据(训练集和测试集)时间区间: self.training_timerange
    3. 获取当前批次验证集数据量: len_backtest_df
    4. 通过FreqaiDataKitchen设置模型路径和名称
    5. 计算策略中的特征数据: dataframe
    6. 获取当前批次训练数据(训练集和测试集): dataframe_base_train
    7. 根据训练数据(训练集和测试集)计算特征标签: strategy.set_freqai_targets
    8. 获取验证集数据并计算标签: dataframe_base_backtest
    9. 调整训练数据(训练集和测试集)和验证集数据的起始位置: dk.slice_dataframe
    10. 模型训练: self.train, 跳转到"BaseRegressionModel.train"
    11. 绘制特征重要性: plot_feature_importance
    12. 保存模型: self.dd.save_data
    13. 模型预测: self.predict
    14. 将验证集标签, 预测值和DI值合并: dk.get_predictions_to_append
    15. 将训练数据和验证数据合并: dk.append_predictions
    16. 保存预测结果到本地: dk.save_backtesting_prediction
- 利用回测数据, 预测未来标签: backtesting_fit_live_predictions
- 合并所有的价格, 特征, 标签和预测数据: fill_predictions

#### data_cleaning_train

最后的训练数据处理

- inlier_metric_window: 衡量当前数据点与最近的历史数据点的相似程度
- principal_component_analysis: PCA降维
- use_SVM_to_remove_outliers: SVM移除异常值
- DI_threshold: 使用DI过滤异常值
- use_DBSCAN_to_remove_outliers: 使用DBSCAN聚类识别异常值
- noise_standard_deviation: 向训练特征中添加噪声防止数据过拟合

#### data_cleaning_predict

最后的验证数据处理

#### backtesting_fit_live_predictions

利用回测数据, 预测未来标签
