预定特定时间的机票，如何抓住机票价格低点购买？
猜测机票价格曲线不是从低到高而是V型曲线。找数据验证猜测并探索价格底部特点，为购票提供经验。

分析对象为 2020年国庆前后一段时间，北京至乌鲁木齐机票价格数据
半个月时间，每天定时启动爬虫，从携程网站抓取当天向后两周内每天该航段航班价格，存入数据库。
原始数据 flights
data_review 展现数据概况
flight_prices 待分析数据全集

生成框架 flight_prices，其行标 index 为被分析对象，列标为被分析对象的值和所有相关特征项。
方法1 序列去重+For循环 ( flights.ipynb)
通过DataFrame列标直接获取某列的Series，.drop_duplicates() 去重（ 对去重后的Series做for 循环）
通过 .loc[ '某列' == xx] 筛选 DataFrame 的数据，形成一个符合过滤要求的 DataFrame
以上两步可反复多次，获得最终的 结果Series。空的DataFrame，通过.append() 添加 结果Series

方法2 groupby + apply（flightsV2.ipynb）
方法1本质上也是在做 “split-apply-combine”的工作，split是通过for循环过滤出一个DataFrame，combine是通过.append()在一个空的DataFrame添加，不够简洁。
使用groupby后，可以显著减少循环的使用，更简洁的完成“split-apply-combin”，例如
flight_prices = flights.groupby(flights.d_time).apply(prices)  # 生成每个航班的票价查询历史
