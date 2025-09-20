---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7FARLRT%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T180049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIBeDmklNDxZ8%2FFOUTo%2Bcj5%2F9cVnb460A4OqQxp64GkiDAiBVJKgDnyW%2BuWRp673DxXJSCyfNRU3t0ul%2FYREba3GODiqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BsoWh%2FsHzSUuXvnlKtwD8UqnWO7rDqJHwhBXoRkpByDxiwa%2BiyxOelSqyEws%2F7vN%2BZC6rIZA9Za7hIFK5oQmDn0Pua%2FGkzKZN3wRl6y9LPoZ%2Foqk9Clh2TvjkDlTQChX%2FmD3FauyFzHz%2F3OcaITcX9gm%2BMruVi1Rc7WLg1REmcGrqqQGuojr9CWm%2BJJlJTrvHkOXzZvkd9HzslcJItYLnqtV3iLdb%2BwMoHwYQauIdB9x4QlyIBOmyqfO9QumKnS6mAqHg2Voeo6JGwmlGFm4KVOHCcts778wTWh6RlXAMVDdkI4HvOH%2BEzdC7QBvY3Q7i1sT9dbOySPkBIWYFAPWXKs%2BiXLGlko32o65XIL6uNDUbq7KlRtDeFUFQXPxy96OXE87ujVCOxZs8tNrJkCUkkO6mB8Hr6A8MyGbk0M%2FxQrkSkssG2ul4Mx4s4DTKmrkqqLqjE4pbiVt0%2Fj25R4MzPvcSSMg6KgOAiyoisoow7Qauo0xQ5vT900Zxv5vwof%2BB1Q41w21xQQuQMHsL%2F8kit%2FWFva7wQ9jNJlmfP35v9zOKlNX5%2F9mFit5yZS9CaOEV1cm1NvqajHOXF%2BlrF3MAHhy0%2BXFvzDdiL%2F5Y7sFOJe1Yf0O9LOmHcd%2Fg5TPsG5fLIODincD3TEIBKswu8u6xgY6pgHLcVlkAOyKy5YIY0Xwt4f6obIUvwG3I8JjDuuodb%2FNvR8LM7AJUK2A2sdPtPau37kLryBGocp1i2vb8kjg5mwmY9h6fBk38IFfpdbHIiJtAap4eG155iqCF%2FF%2FK00fIeT2kVMlu7aPb%2FxOcrFQ%2BQhZwm1jL8QAGn%2FY%2BKTtmKYFCtirhET5JjNp8Q9VMIxps5vc8EMFtHGYPoSn6H8hTy9hivWFmZfq&X-Amz-Signature=87ca475d156a74ea85edaa10119fcb895b5d682bc089282d21999985ff58714c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

