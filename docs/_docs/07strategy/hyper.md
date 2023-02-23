---
title: hyper.py
category: strategy
order: 2
---

## HyperStrategyMixin

#### 初始化

1. 获取配置: self.config = config
2. 买入参数列表: self.ft_buy_params: List[BaseParameter] = []
3. 卖出参数列表: self.ft_sell_params: List[BaseParameter] = []
4. 保护参数列表: self.ft_protection_params: List[BaseParameter] = []

5. 从文件中加载参数: params = self.load_params_from_file()
params = params.get('params', {})
self._ft_params_from_file = params

#### 方法

1. enumerate_parameters: 获取指定参数列表生成器
2. detect_all_parameters: 获取全部参数列表
3. ft_load_params_from_file: 加载止盈止损参数
4. ft_load_hyper_params: 加载买卖和保护参数
5. load_params_from_file: 从文件中加载参数
6. get_no_optimize_params: 返回不属于当前优化的作业列表

## detect_parameters
检测参数名字和参数值

