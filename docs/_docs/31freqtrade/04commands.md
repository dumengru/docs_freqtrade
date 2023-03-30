---
title: commands
category: freqtrade
order: 4
---

## cli_options.py
命令选项文件, 所有可执行命令选项都在**AVAILABLE_CLI_OPTIONS**字典中

## arguments.py

> argparse 模块是 Python 内置的用于命令项选项与参数解析的模块

命令参数文件, 所有操作相关命令及参数设置都在**Arguments**类中

#### Arguments
- self.args: 输入的参数列表
- self._parsed_arg

**get_parsed_arg**
- 创建所有命令组: _build_subcommands
- 解析命令: _parse_args

**_build_subcommands**

创建所有命令组, 所有命令都会设置默认执行参数
```python
func=...
```
执行命令时就执行...函数

**_build_args**
将所有optionlist命令添加到parser命令组中

**_parse_args**
1. 如果不设置工作目录, 默认工作目录是"user_data"
2. 默认配置文件"user_data/config.json", 如果不存在则使用"config.json"
3. 返回所有的参数配置

## trade_commands.py
- start_trading: 启动交易

## build_config_commands.py
- start_new_config: 创建新的模板配置文件

## deploy_commands.py
- start_create_userdir: 创建默认文件夹
- start_new_strategy: 创建新的模板策略文件
- start_install_ui: 启动安装ui工具

## data_commands.py
- start_download_data: 开始下载数据
- start_convert_data: 数据格式转换
- start_convert_trades: 
- start_list_data: 展示可回测(已下载)的数据

## optimize_commands.py
- start_backtesting: 启动回测
- start_backtesting_show: 展示回测结果
- start_edge: 启动策略优化
- start_hyperopt: 启动参数优化

## analyze_commands.py
- start_analysis_entries_exits: 进行回测分析

## hyperopt_commands.py
- start_hyperopt_list: 展示参数优化结果
- start_hyperopt_show: 展示详细的参数优化结果

## list_commands.py
- start_list_exchanges: 展示所有交易所
- start_list_markets: 展示所有可交易品种
- start_list_strategies: 展示所有可用策略
- start_list_freqAI_models: 展示所有可用的AI模型
- start_list_timeframes: 展示所有可交易的时间周期
- start_show_trades: 展示交易结果

## pairlist_commands.py
- start_test_pairlist: 启动货币对测试

## db_commands.py
- start_convert_db: 启动数据库迁移

## plot_commands.py
- start_plot_dataframe: 启动绘制数据
- start_plot_profit: 启动绘制利润

## webserver.py
- start_webserver: 启动webserver
