---
title: configuration
category: freqtrade
order: 5
---

## configuration.py

#### Configuration
配置对象

- self.args: 命令参数
- self.config: 配置参数
- self.runmode: 操作类型

- get_config: 获取所有配置
- load_config: 加载配置
    - 如果配置了**original_config**, 会先加载
    -_process_logging_options: 配置日志
    - _process_runmode: 配置回测模式
    - _process_common_options: 配置命令参数
    - _process_trading_options: 根据交易模式配置数据库
    - _process_optimize_options: 配置所有策略优化相关参数
    - _process_plot_options: 配置所有绘图相关参数
    - _process_data_options: 配置数据相关参数
    - _process_analyze_options: 配置策略分析相关参数
    - _process_freqai_options: 配置AI模型参数
    - check_exchange: 检查交易所是否被CCXT支持
    - _resolve_pairs_list: 交易货币对配置
    - process_temporary_deprecated_settings: 配置telegram

