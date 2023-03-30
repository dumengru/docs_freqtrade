## exchange.py

实际是将很多同步操作包装为异步操作

**Exchange**
1. 设置属性: synchronous = False
> 初始化
1. 添加异步相关配置
2. 创建节流器对象Throttler
> open
- 打开aiohttp.ClientSession
> close
- 关闭aiohttp.ClientSession

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
