
**创建目录**
```python
freqtrade create-userdir

--userdir PATH
--reset                                 # 是否覆盖旧目录(谨慎使用)
```

**创建新配置**
```python
freqtrade new-config

--config PATH                           # 指定配置目录
```

**创建新策略**
```python
freqtrade new-strategy

--userdir PATH                          # 指定策略目录
--strategy NAME                         # 指定策略名称
--template {full,minimal,advanced}      # 指定策略模板
```

**查看策略**
```python
freqtrade list-strategies

--strategy-path PATH                    # 搜索指定路径
--recursive-strategy-search             # 在文件夹中递归搜索策略
```

**列出交易所**
```python
freqtrade list-exchanges

-1                                      # 1列显示
-a                                      # 列出所有ccxt中的交易所
```

**列出时间框架**
```python
freqtrade list-timeframes

--exchange EXCHANGE                    # 指定交易所
--userdir                               # 指定文件夹
```

**查看可交易货币对**
```python
freqtrade list-markets                      # 也可用 list-pairs

--exchange EXCHANGE                         # 指定交易所
--base BASE_CURRENCY                        # 指定基础货币
--quote QUOTE_CURRENCY                      # 指定报价货币
--trading-mode {spot,margin,futures}        # 指定交易模式
--all                                       # 显示全部
```

**测试货币对**
```python
freqtrade test-pairlist
--config PATH                           # 指定配置文件
--quote QUOTE_CURRENCY                  # 指定报价货币
--exchange EXCHANGE                     # 指定交易所
```

**网络服务**
```python
freqtrade webserver
```

**数据下载**
```python
freqtrade download-data

--exchange binance                      # 指定交易所
--pairs ETH/USDT BTC/USDT               # 指定货币对
--datadir user_data/data/               # 指定目录
--pairs-file other_dir/pairs.json       # 指定货币对json文件
--days 10                               # 下载最近10天数据(默认30)
--timeframes 1m 5m                      # 指定时间框架
--timerange 20220101-                   # 指定时间范围
--trading-mode {spot,margin,futures}    # 指定交易模式

-c/--config                             # 指定配置文件
```

`freqtrade download-data --exchange binance --pairs BTC/USDT ETH/USDT --timeframes 1m 5m 15m 30m 1h 1d --timerange 20210101- --trading-mode futures`

注意事项: 
1. .*/USDT 可下载所有活跃的USDT货币对
2. --timerange 20220101- 下载2022年之后所有数据
3. pairs.json文件格式(交易所不支持的货币对会被忽略)
```python
[
    "ETH/BTC",
    "ETH/USDT",
    "BTC/USDT",
    "XRP/ETH"
]
```

**回测命令**
```python
freqtrade backtesting 

--timeframe TIMEFRAME
--timerange TIMERANGE
--strategy-list STRATEGY_LIST       # 指定策略列表
--strategy NAME                     # 指定策略名称
--strategy-path PATH                # 指定策略路径
--export {none,trades,signals}      # 指定导出内容
--export-filename PATH              # 指定导出路径
--cache none
```

**显示回测结果**
```python
freqtrade backtesting-show

--export-filename PATH              # 指定回测结果目录
--show-pair-list                    # 按利润将货币对排序
```

**显示详细的回测分析**
```python
freqtrade backtesting-analysis

--export-filename PATH                      # 回测结果目录
--analysis-groups {0,1,2,3,4}               # 分组汇总
--enter-reason-list                         # 指定输出买入标签
--exit-reason-list                          # 指定输出卖出标签
--indicator-list                            # 指定输出指标

--eps, --enable-position-stackin            # 同一货币允许多次开仓
--dmmp, --disable-max-market-positions      # 禁用max_open_trades
```

注意事项: 
1. 回测时需要指定参数`--export signals`
2. 分组数字含义
- 1: 按 enter_tag 分组的利润汇总
- 2: 按 enter_tag 和 exit_tag 分组的利润汇总
- 3: 按pair和enter_tag分组的利润汇总
- 4: 按对、enter_ 和 exit_tag 分组的利润摘要(这可能会很大)

**参数优化**
```python
freqtrade hyperopt

--timeframe TIMEFRAME           # 回测时间框架
--timerange TIMERANGE           # 回测时间范围
--hyperopt-loss NAME            # 优化目标
--analyze-per-epoch             # 每次回测都计算`populate_indicators `
--strategy NAME                 # 策略名称
--strategy-path PATH            # 策略路径
--random-state                  # 随机种子(参数优化过程中有随机数)
--print-all                     # 输出全部结果(默认输出最优的)  

```
1. 优化目标包括: 
- ShortTradeDurHyperOptLoss
- OnlyProfitHyperOptLoss
- SharpeHyperOptLoss
- SharpeHyperOptLossDaily
- SortinoHyperOptLoss
- SortinoHyperOptLossDaily
- CalmarHyperOptLoss
- MaxDrawDownHyperOptLoss
- MaxDrawDownRelativeHyperOptLoss
- ProfitDrawDownHyperOptLoss
2. space选项
- all
- buy
- sell
- roi
- stoploss
- trailing
- protection
- default: all except trailing and protection

2. 快速优化roi, stoploss 和 trailing 命令
- `freqtrade hyperopt --hyperopt-loss SharpeHyperOptLossDaily --spaces roi stoploss trailing --strategy TemplateMinimal --config config.json -e 100 --timerange 20220501-`
2. 优化 protections 命令
- `freqtrade hyperopt --hyperopt-loss SharpeHyperOptLossDaily --strategy TemplateHyperopt --spaces protection --config config.json -e 100 --timerange 20220501-`

**显示参数优化结果**
```python
freqtrade hyperopt-list

--hyperopt-filename             # 回测结果文件
--export-csv FILE               # 导出回测结果到csv文件
```

**显示详细的参数优化结果**
```python
freqtrade hyperopt-show

-n INT, --index INT             # 选择某条详细信息
```
1. 建议先使用 `freqtrade hyperopt-list` 浏览全部回测结果
2. 然后使用 `freqtrade hyperopt-show -n INT` 获取某条详细信息

**显示交易信息**
```python
freqtrade show-trades

--db-url PATH                   # 指定数据库路径
--trade-ids TRADE_IDS           # 指定交易id
--print-json                    # 输出为json
```

**实盘命令**
```python
freqtrade trade
--config PATH                   # 配置文件
--strategy NAME                 # 策略名称

--strategy-path PATH            # 策略路径
--userdir PATH                  # 数据路径
--db-url PATH                   # 数据库路径
```

**绘图K线**
```python
freqtrade plot-dataframe

-p PAIRS [PAIRS ...]            # 货币对列表
--strategy NAME                 # 策略名称
--strategy-path PATH            # 策略路径
--timerange TIMERANGE           # 时间区间
--timeframe TIMEFRAME           # 时间周期
--export-filename PATH          # 导出文件
--export EXPORT                 # 导出回测结果
```
1. 绘制存储在数据库中的交易
`freqtrade plot-dataframe --strategy AwesomeStrategy --db-url sqlite:///tradesv3.dry_run.sqlite -p BTC/USDT --trade-source DB`
2. 根据回测结果绘制交易
`freqtrade plot-dataframe --strategy AwesomeStrategy --export-filename user_data/backtest_results/backtest-result.json -p BTC/USDT --timerange 20220101-20220501`

**绘图收益**
```python
freqtrade plot-profit

-p PAIRS [PAIRS ...]            # 货币对列表
--strategy NAME                 # 策略名称
--strategy-path PATH            # 策略路径
--timerange TIMERANGE           # 时间区间
--timeframe TIMEFRAME           # 时间周期
--export-filename PATH          # 导出文件
--export EXPORT                 # 导出回测结果
```
1. 使用自定义数据库
`freqtrade plot-profit  -p BTC/USDT --db-url sqlite:///tradesv3.sqlite --trade-source DB`
2. 使用自定义回测导出文件
`freqtrade plot-profit  -p BTC/USDT --export-filename user_data/backtest_results/backtest-result.json --timerange 20220101-20220501 --timeframe 1h`

****
```python

```

