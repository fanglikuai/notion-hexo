---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665XESB4BK%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCIEuBLAft5nAgL5xidRkZPHyahdoxx2AEsdFcmiTqvNuLAiA2%2B6S%2Fo6xMQAi5NjEjCFodrkbilGDugRxEljrvA79DMyr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIMX2GraFFr8QdDTDBfKtwDdUNTxNYi6bHY4VFBMlz0ZGPcc4mY5hE2dqLFRtD36sO6cJW9e50CnLLHd7uDvXOHN9pLQuOgmdf2JpOQ6GAgYPigBb6O0DI%2BILiq1ZFljjrUO7ZaQJdvbPHpPx9xWT3b0AgTV6MVVVXQtNai43HYIVVxWTkykP%2B%2BR%2BdZJnHBuNayYg70Tc5tztT7%2FYKBuDlfmvUxE1zXFYN0Pd2dl2DmM8NoaHYVrMNw22ZF0EoP8bdsnc8DiTCiUwzJKrwCfbmtEQB21H49qlqsi5iuVhMvBF1oIGEr%2BN5zzrtFi09HsKSkAiiz97tHVTqJirLc9%2BtZk4h29cVAIhJyvJ8cWEphAVmwzbHStQ%2FphR6Kz2Rgd2%2FeEak5pFXQc%2FzSa4JsWxEaBL%2FX%2Blm0WHb93T%2FuhWJYDXk9QeKbZ1kvhSbNdsysOwWmW86oCK24Sa8nfgo2Zono%2FT4X7c1cJ9jiUQOKq6Au37eA1mC1Hlz4m2oblKWZVxytyk4NdBvE%2BcfX5Gahkd2YaxKgoTUZPniu1V%2FsMzZ%2B%2F3oZ7yaDh2HuLHHIkPtEzKtNrxOoHcWE8JQrL%2B2Rq%2FNpltaurP4e5gJ7KsQPAmWfbtGAZX3HzJbCaT%2B%2FHMVzmSFw6zSjrSOlK%2Fe%2FUA4w2OjgxwY6pgEjBCE3NR45kKOf8ZqkredyEOy5MxK6lvC9f9%2BG%2BJ8yn4LbNRJSjCiQq4Bv29pn7qf7xIJWmlRgBzO6C2FY%2BiNOBBMJka6tY924jbp7uRTWLPPF3Td5rectkVsjkFLz%2BkY4ICZgEWNhYXImZ9XkocSk%2FWx9oPIDcQhP49BUj9qZiiR98KB%2FbFPjxQt6bEvf2AFZDBzJyYx42fBYCqZIerY9acXx1sqg&X-Amz-Signature=4afa3a24a42bd65a54a386e5a794bb54da8003880d628af76945eea3000caf3e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

