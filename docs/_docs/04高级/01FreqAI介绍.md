---
title: FreqAI介绍
category: 高级
order: 1
---

![](../../images/202302281701.png)

## 简介

FreqAI是利用机器学习, 深度学习和强化学习的相关算法构建AI模型并用于交易的软件

AI相关参考文档:

- scikit-learn文档: https://scikit-learn.org/stable/user_guide.html
- Keras文档: https://keras.io/zh/
- PyTorch文档: https://www.pytorchtutorial.com/docs/

- 动手学深度学习: http://zh.d2l.ai/
- 动手学强化学习: https://hrl.boyuai.com/chapter/intro

- stable-baselines3: https://stable-baselines3.readthedocs.io/en/master/index.html
- OpenAI gym: https://www.gymlibrary.dev/content/basic_usage/

#### 安装

```python
pip install -r requirements-freqai.txt
```

#### 启动示例

```python
freqtrade trade --config config_examples/config_freqai.example.json --strategy FreqaiExampleStrategy --freqaimodel LightGBMRegressor --strategy-path freqtrade/templates
```

- 示例策略: freqtrade/templates/FreqaiExampleStrategy.py
- 预测模型: freqtrade/freqai/prediction_models/LightGBMRegressor.py
- 示例配置: config_examples/config_freqai.example.json

## 配置

1. 货币对过滤器选择: StaticPairList
2. 至少需要包含以下配置

```json
"freqai": {
    "enabled": true,                # 是否启用freqai
    "purge_old_models": true,       # 清除旧模型
    "train_period_days": 30,        # 训练集数据量(天)
    "backtest_period_days": 7,      # 测试集数据量(天)
    "identifier" : "unique-id",     # 模型id
    # 特征因子配置
    "feature_parameters" : {
        "include_timeframes": ["5m","15m","4h"],    # 包括的时间周期
        # 包含的货币对
        "include_corr_pairlist": [
            "ETH/USDT",
            "BTC/USDT",
            "BNB/USDT"
        ],
        "label_period_candles": 24,
        "include_shifted_candles": 2,
        "indicator_periods_candles": [10, 20]
    },
    # 数据中的测试集比例
    "data_split_parameters" : {
        "test_size": 0.25
    }
}
```

## 策略方法

#### feature_engineering_expand_all

1. 该方法会自动根据配置中的"indicator_periods_candles"(指标周期), "include_timeframes"(特征包含时间周期), "include_shifted_candles"(数据平移周期), "include_corr_pairs"(有相关性的货币对)四个参数构造不同组合的特征
2. 所有特征必须以"%"开头

```python
def feature_engineering_expand_all(self, dataframe, period, **kwargs):
    dataframe["%-rsi-period"] = ta.RSI(dataframe, timeperiod=period)
    dataframe["%-mfi-period"] = ta.MFI(dataframe, timeperiod=period)
    dataframe["%-adx-period"] = ta.ADX(dataframe, timeperiod=period)
    dataframe["%-sma-period"] = ta.SMA(dataframe, timeperiod=period)
    dataframe["%-ema-period"] = ta.EMA(dataframe, timeperiod=period)

    return dataframe
```

#### feature_engineering_expand_basic

1. 该方法会自动根据配置中的"include_timeframes"(特征包含时间周期), "include_shifted_candles"(数据平移周期), "include_corr_pairs"(有相关性的货币对)四个参数构造不同组合的特征
2. 所有特征必须以"%"开头

```python
def feature_engineering_expand_basic(self, dataframe, **kwargs):
    dataframe["%-pct-change"] = dataframe["close"].pct_change()
    dataframe["%-raw_volume"] = dataframe["volume"]
    dataframe["%-raw_price"] = dataframe["close"]
    return dataframe
```

#### feature_engineering_standard

1. 该方法构造一些无法通过组合处理的特征
2. 所有特征必须以"%"开头

```python
def feature_engineering_standard(self, dataframe, **kwargs):
    dataframe["%-day_of_week"] = (dataframe["date"].dt.dayofweek + 1) / 7
    dataframe["%-hour_of_day"] = (dataframe["date"].dt.hour + 1) / 25
    return dataframe
```

#### set_freqai_targets

1. 该方法设置预测的标签值
2. 标签必须以"&"开头

```python
def set_freqai_targets(self, dataframe, **kwargs):
    dataframe["&-s_close"] = (
        dataframe["close"]
        .shift(-self.freqai_info["feature_parameters"]["label_period_candles"])
        .rolling(self.freqai_info["feature_parameters"]["label_period_candles"])
        .mean()
        / dataframe["close"]
        - 1
        )
```

#### populate_indicators

1. 启动模型
2. "self.freqai.start()"函数只能在"populate_indicators"方法中使用

```python
def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
    dataframe = self.freqai.start(dataframe, metadata, self)
    return dataframe
```

## 特征模式

#### df['&*']

所有以"&"开头的数据列都会被视为预测的标签, 例如假设要预测未来第40根K线收盘价

1. 在配置中设置: "label_period_candles": 40
2. 在"set_freqai_targets"方法中编写

```python
df['&-s_close'] = df['close'].shift(-self.freqai_info["feature_parameters"]["label_period_candles"])
```

3. 在"populate_entry/exit_trend"方法中编写根据"df['&-s_close']"判断的入场或离场逻辑

#### df['&*_std/mean']

训练区间定义标签的标准差和均值

#### df['do_predict']

判断预测值是否异常, 数据值在-2到2之间, "do_predict" == 1表示数据是可信的

#### df['DI_values']

差异指数, DI的值表示FreqAI预测的置信度

#### df['%*']

所有以"%"开头的数据列都被视为特征

## 模型目标

有监督的机器学习模型一般分两大类: 回归和分类, 回归主要预测值(即价格未来会是多少), 分类主要预测方向(即价格是上涨还是下跌)

1. 如果我们使用的是回归模型, 则预测未来100根K线收盘价写法如下

```python
df['&s-close_price'] = df['close'].shift(-100)
```

2. 如果我们使用的是分类模型, 则预测未来100根K线涨跌写法如下

```python
df['&s-up_or_down'] = np.where( df["close"].shift(-100) > df["close"], 'up', 'down')
df['&s-up_or_down'] = np.where( df["close"].shift(-100) == df["close"], 'same', df['&s-up_or_down'])
```

## 策略示例

该示例在"freqtrade/templates/FreqaiExampleStrategy.py"文件中

```python
class FreqaiExampleStrategy(IStrategy):
    # 定义ROI
    minimal_roi = {"0": 0.1, "240": -1}
    # 定义绘图配置: 附图指标, 收盘价预测值蓝色, 异常值棕色
    plot_config = {
        "main_plot": {},
        "subplots": {
            "&-s_close": {"prediction": {"color": "blue"}},
            "do_predict": {
                "do_predict": {"color": "brown"},
            },
        },
    }
    # 只在新K线出现时计算
    process_only_new_candles = True
    # 止损
    stoploss = -0.05
    # 启用退出信号: custom_exit
    use_exit_signal = True
    # 起始K线数量
    startup_candle_count: int = 40
    # 是否可做空
    can_short = True
    # 自定义参数空间
    std_dev_multiplier_buy = CategoricalParameter(
        [0.75, 1, 1.25, 1.5, 1.75], default=1.25, space="buy", optimize=True)
    std_dev_multiplier_sell = CategoricalParameter(
        [0.75, 1, 1.25, 1.5, 1.75], space="sell", default=1.25, optimize=True)

    def feature_engineering_expand_all(self, dataframe: DataFrame, period: int,
                                       metadata: Dict, **kwargs):
        # 计算指标作为特征, 所有特征以%开头
        dataframe["%-rsi-period"] = ta.RSI(dataframe, timeperiod=period)
        dataframe["%-mfi-period"] = ta.MFI(dataframe, timeperiod=period)
        dataframe["%-adx-period"] = ta.ADX(dataframe, timeperiod=period)
        dataframe["%-sma-period"] = ta.SMA(dataframe, timeperiod=period)
        dataframe["%-ema-period"] = ta.EMA(dataframe, timeperiod=period)

        bollinger = qtpylib.bollinger_bands(
            qtpylib.typical_price(dataframe), window=period, stds=2.2
        )
        dataframe["bb_lowerband-period"] = bollinger["lower"]
        dataframe["bb_middleband-period"] = bollinger["mid"]
        dataframe["bb_upperband-period"] = bollinger["upper"]

        dataframe["%-bb_width-period"] = (
            dataframe["bb_upperband-period"]
            - dataframe["bb_lowerband-period"]
        ) / dataframe["bb_middleband-period"]
        dataframe["%-close-bb_lower-period"] = (
            dataframe["close"] / dataframe["bb_lowerband-period"]
        )

        dataframe["%-roc-period"] = ta.ROC(dataframe, timeperiod=period)

        dataframe["%-relative_volume-period"] = (
            dataframe["volume"] / dataframe["volume"].rolling(period).mean()
        )

        return dataframe

    def feature_engineering_expand_basic(self, dataframe: DataFrame, metadata: Dict, **kwargs):
        # 添加收益率, 成交量, 收盘价为特征
        dataframe["%-pct-change"] = dataframe["close"].pct_change()
        dataframe["%-raw_volume"] = dataframe["volume"]
        dataframe["%-raw_price"] = dataframe["close"]
        return dataframe

    def feature_engineering_standard(self, dataframe: DataFrame, metadata: Dict, **kwargs):
        # 添加天数和小时数作为特征
        dataframe["%-day_of_week"] = dataframe["date"].dt.dayofweek
        dataframe["%-hour_of_day"] = dataframe["date"].dt.hour
        return dataframe

    def set_freqai_targets(self, dataframe: DataFrame, metadata: Dict, **kwargs):
        # 添加预测标签, 预测标签以&开头
        dataframe["&-s_close"] = (
            dataframe["close"]
            .shift(-self.freqai_info["feature_parameters"]["label_period_candles"])
            .rolling(self.freqai_info["feature_parameters"]["label_period_candles"])
            .mean()
            / dataframe["close"]
            - 1
            )
        return dataframe

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        # 启动freqai(完成训练和预测)
        dataframe = self.freqai.start(dataframe, metadata, self)
        # 计算预测收盘价的: 均值 +/- N倍标准差
        for val in self.std_dev_multiplier_buy.range:
            dataframe[f'target_roi_{val}'] = (
                dataframe["&-s_close_mean"] + dataframe["&-s_close_std"] * val
                )
        for val in self.std_dev_multiplier_sell.range:
            dataframe[f'sell_roi_{val}'] = (
                dataframe["&-s_close_mean"] - dataframe["&-s_close_std"] * val
                )
        return dataframe

    def populate_entry_trend(self, df: DataFrame, metadata: dict) -> DataFrame:
        # 添加入场做多信号
        enter_long_conditions = [
            df["do_predict"] == 1,
            df["&-s_close"] > df[f"target_roi_{self.std_dev_multiplier_buy.value}"],
            ]

        if enter_long_conditions:
            df.loc[
                reduce(lambda x, y: x & y, enter_long_conditions), ["enter_long", "enter_tag"]
            ] = (1, "long")
        # 添加入场做空信号
        enter_short_conditions = [
            df["do_predict"] == 1,
            df["&-s_close"] < df[f"sell_roi_{self.std_dev_multiplier_sell.value}"],
            ]

        if enter_short_conditions:
            df.loc[
                reduce(lambda x, y: x & y, enter_short_conditions), ["enter_short", "enter_tag"]
            ] = (1, "short")

        return df

    def populate_exit_trend(self, df: DataFrame, metadata: dict) -> DataFrame:
        # 添加做多离场信号
        exit_long_conditions = [
            df["do_predict"] == 1,
            df["&-s_close"] < df[f"sell_roi_{self.std_dev_multiplier_sell.value}"] * 0.25,
            ]
        if exit_long_conditions:
            df.loc[reduce(lambda x, y: x & y, exit_long_conditions), "exit_long"] = 1
        # 添加做空离场信号
        exit_short_conditions = [
            df["do_predict"] == 1,
            df["&-s_close"] > df[f"target_roi_{self.std_dev_multiplier_buy.value}"] * 0.25,
            ]
        if exit_short_conditions:
            df.loc[reduce(lambda x, y: x & y, exit_short_conditions), "exit_short"] = 1

        return df

    def confirm_trade_entry(
        self,
        pair: str,
        order_type: str,
        amount: float,
        rate: float,
        time_in_force: str,
        current_time,
        entry_tag,
        side: str,
        **kwargs,
    ) -> bool:
        # 自定义确认交易结果

        # 获取最新的K线数据
        df, _ = self.dp.get_analyzed_dataframe(pair, self.timeframe)
        last_candle = df.iloc[-1].squeeze()
        # 如果当前交易方向为做多, 且委托价格大于最新收盘价的1.0025倍则不交易
        if side == "long":
            if rate > (last_candle["close"] * (1 + 0.0025)):
                return False
        # 如果当前交易方向为做空, 且委托价格小于最新收盘价的0.9975倍则不交易
        else:
            if rate < (last_candle["close"] * (1 - 0.0025)):
                return False

        return True
```
