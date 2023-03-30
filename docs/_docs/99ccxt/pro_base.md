## order_book_side.py
**OrderBookSide**
- 继承list, 存放bids或asks数据

**CountedOrderBookSide**
- 在OrderBook中包含price, size, count三个值

**IndexedOrderBookSide**
- 在OrderBook中包含price, size, order_id三个值

**自定义类**
1. Asks列表
- Asks
- CountedAsks
- IndexedAsks
2. Bids列表
- Bids
- CountedBids
- IndexedBids

## order_book.py
**OrderBook**
- 继承dict, 相当于自定义OrderBook字典

**CountedOrderBook**
- Counted类型OrderBook字典

**IndexedOrderBook**
- Indexed类型OrderBook字典

## exchange.py

> inflate/inflate64/gunzip
- 压缩/解压数据
> order_book/indexed_order_book/counted_order_book
- 返回OrderBook对象
> client
- 创建FastClient
> spawn
- 执行异步方法
> delay
- 等待之后执行异步方法
> on_error
- 删除异常客户端
> on_close
- 删除客户端
> close
- 关闭客户端
> watch_...
- 提供异步WebSocket接口
