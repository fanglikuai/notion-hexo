---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYOLKFHL%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T200131Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD864pbWMPPHWYsJng%2BdXl4lLysMYfciPnlJzPG2KEBmQIhAJvPj%2FlCszgF8aXU%2FXM%2BmBUC8aGUxkRzt9%2Bao1sIXhagKv8DCG0QABoMNjM3NDIzMTgzODA1IgzY7ziP1ojNZQI%2FGaEq3AP5G2KCrzFs%2BP3HJvJY4sAgbQo7r0j6j8auqQ5FnzZOwQ0JBTT2%2Ft88BcdDBCb%2BjS2rhP6VVzDeiMNGoPHFs5q1gTZvyWJz5ZYnk9GiJ1HEEqH%2BYXrx45u%2BKiKGKL6ksgntcdx%2FjHP0usohM8QTa9S9ApvCX860IoYQA8i0IVLacZo7%2F53mJCM7eynsbvo86YqxW3SxebvCvlYyBIJJ1ye0PZtdpuK%2Fn%2BUPmGXSSg%2BG8Vpx%2Bqw%2Bq5t0EDXaMKAMYql29Lb9RuRptYQE%2F%2BLagr8QkcbrmZtFzEMjLezopSfQXxuWUU4pIW%2FWV86QzhWMucSRtR1%2FEf0TN7ZY54JmvHrcTO4ftU%2Ft3sZZt9znAz1yXptJrEsm5olSBtETNbBu20dD3T9l%2FW0OiEjXiLw%2FjF4XiAS99wLFmNXipHIpjb9Q8iljI0pPzhLMpVt1FE0lj4AmpllH8IEBa9CsnTPAuT9vdsHz9hwcypKXTKEOfzQtP1pR2nQzzTQujObXM2eBegKUTCNPwne2SbU0Y0zdtY%2FlGR%2FbKx6Zb1Pt9kUkH5qWQM8KQ9fQO40Zw6MzN0AOWMN5nxa6tS5c4hrPyVhj15tA2KfRy6a4YQm5INFBwYfjN5DIWK6Iql23vSXJejDOkd7IBjqkAevzTED%2B%2BRKDgv7Bkrl%2FZgfCOSqoEufUGqPDhy198U23OkQbwmyxJOysKFR6I36bw9pqyGFUX%2B9TM35Ml%2F04oueT6hsYyxg7i8pM0WYoNWYsIjqAHTGKx1WtY1sdHQMMdPH1Iiq9sh1xGXYVBeynMa9OW%2BG6rpm2FjJq8vjKMHkiTRdZquwW4xrjLwHJULwQmqHD3RaDDmtnYqKdlr%2BJ9Ob0IjxK&X-Amz-Signature=ede48b3fa94827649b782aae0b675555ce3ffd98b2bed4d62e0ce86517d005e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 23:53:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
banner_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
---

# bigkey 问题


![1753077336565-23eda3f0-dd0d-4865-9f4e-b536a19e7c9b.png](/images/c6758344cbe13f3ebf0f8718f40ab3f3.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h 10.66.64.84 -p 10229 -a xxxx --bigkeys`
- rdb 文件扫描
- 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key

# 大key进行拆分


采用经典算法“分治法”，将大而化小。针对String和集合类型的Key，可以采用如下方式：

- String类型的大Key：可以尝试将对象分拆成几个Key-Value， 使用MGET或者多个GET组成的pipeline获取值，分拆单次操作的压力，对于集群来说可以将操作压力平摊到多个分片上，降低对单个分片的影响。
- 集合类型的大Key，并且需要整存整取要在设计上严格禁止这种场景的出现，如无法拆分，有效的方法是将该大Key从JIMDB去除，单独放到其他存储介质上。
- 集合类型的大Key，每次只需操作部分元素：将集合类型中的元素分拆。以Hash类型为例，可以在客户端定义一个分拆Key的数量N，每次对HGET和HSET操作的field计算哈希值并取模N，确定该field落在哪个Key上。

### 缺点


本质就是取模，需要在客户端进行操作，限定取模的数量，不够灵活

