## exchange.py
**Exchange**
- 交易所基类
> 类属性
- 定义交易所需要的各种基本属性
> 初始化
1. 定义部分属性默认类型
2. 将Exchange.describe和传入的config参数变为属性
3. 设置Exchange.markets
4. 给所有的属性都添加一个驼峰命名法别名
5. 其他细节
> set_sandbox_mode
- 启动测试网络
> define_rest_api_endpoint
- 
> define_rest_api
- 
> throttle
- 
> gzip_deflate
- 
> prepare_request_headers
- 准备请求头
> is_text_response
- 通过请求头判断请求数据是否文本类型
> key_exists
- 判断键是否存在
> currency
- 获取品种currency
> market
- 获取品种市场信息
> ..._precision
- 将...数据转为指定精度
> capitalize
- 首字母大写
> strip
- 字符串去空值
> keysort
- 字典排序
> deep_extend
- 合并数据
> group_by
- 分组数据
> unique
- 数组去重
> dmy
- 指定日期时间连接符
> parse8601
- 解析时间戳
> hash
- 获取has值
> check_address
- 检查地址是否有效
> precision_from_string
- 数字转字符串
> sign
- 签名
> hashMessage
- 对数据进行hash
> signHash
- 签名hash
> calculate_fee
- 计算手续费
> market_ids
- 获取所有品种id
> market_symbols
- 获取l2订单簿
> get_network
- 获取网络
> nonce
- 当前秒时间戳
> set_headers
- 设置请求头
> market_id
- 获取品种id
> symbol
- 获取品种信息

> safe_...
- 将数据转为安全类型/固定格式
> uuid...
- 获取一个uuid
> fetch_...
- 获取...数据
> load_...
- 加载...数据
> parse_...
- 解析...数据
> filter_...
- 过滤...数据
> convert_...
- 转换...数据
> check_...
- 检查...数据
> handle_...
- 处理... 数据

> edit_order
- 修改订单(先撤单, 再重发)
> edit_limit_order
- 修改限价单
> edit_limit_buy_order
- 修改限价买单
> edit_limit_sell_order
- 修改限价卖单
> create_order
- 创建订单
> create_limit_order
- 创建限价单
> create_limit_buy_order
- 创建限价买单
> create_limit_sell_order
- 创建限价卖单
> create_market_order
- 创建市价单
> create_market_buy_order
- 创建市价买单
> create_market_sell_order
- 创建市价卖单
> create_post_only_order
- 创建post_only订单
> create_reduce_only_order
- 创建reduce_only订单
> create_stop_order
- 创建stop单
> create_stop_limit_order
- 创建stop_limit单
> 
- 

## decimal_to_precision.py
**decimal_to_precision**
数据精度转换

## precise.py
**Precise**
数据精度操作


