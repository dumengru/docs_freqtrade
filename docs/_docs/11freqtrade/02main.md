---
title: main.py
category: freqtrade
order: 2
---

## main.py
启动函数: `main`
1. 加载日志配置: setup_logging_pre
2. 创建参数对象: Arguments
3. 解析输入参数并返回参数字典: arguments.get_parsed_arg
4. 如果"func"在参数中, 则执行函数, 否则抛出异常