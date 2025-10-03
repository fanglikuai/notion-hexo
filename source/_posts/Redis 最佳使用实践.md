---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSPJ74NI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T190052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD3mtWe86HNRqBBG%2BIPvJ2fFuIjsfnC6OlakG6RlOvBOwIhAO%2Fu9cUqjpWb4V1HOeHZMn3dDwm9yIojL5SH1e%2FLXa1tKv8DCEsQABoMNjM3NDIzMTgzODA1Igzj7%2BTJAx4gU9HKvh0q3AOguatVliJ8BKdxoFnXM7dTxtPoVFxRqnjtW35elgCRgWHHKTM%2FvHWZxNFvmEmv1mqvbYuAa2xgF%2FHT2EIleMkPbYrZiaUA9L%2F36rbtL6xFIov6w3pwEMcJMWqltTtZB4S8tvfUtAKjRP2mLsOFJ5Ekl2ZRuEDwOSYOJRy%2BoPtjA%2Bg5Wi6BnMhyk5UM9%2BqbS%2FOP6tLPnYIRv9zEObSSmXhnX16IkioyDzVbIPYn8YeSauKZSd7tmyahTIde0CZ2QOuDuO%2Fz4KcYDcQiZKdpdqJvGKbClg4ijL92lo6aRDHuQCxQ%2BPbhEhb%2FYOqfhV%2BjnE4EKkhG8BuOZaOjt8J1ox4a8R4keBR65Z10ZDxRWqPQrWgoVlolEGM1XHJMJOLeLoBiQXjkIzqBo4IAJaBLFPA7umAvYr7HzC9T8b7qkAGXLQ1HmXICApVQXCvLEH8XJFuvUJgmJ733p93MzVbaxzhFFpYDqWKE9WUTObptm4tDD3Afi3AilEmj51X1b1Vvd6EkNl%2B4QvP759c4B0w5GNEKwYUuY0QYizZwOlmUmvo78CFk2YBBGV0%2BIaG7mjdzHiazw2AVlxvR%2FoIPnkH7466FHvB%2Bg%2Bu2pGCX5jBlDnpM214xoWU3yiiwH2iXKDC5l4DHBjqkAWy07QmShhd7z9%2F%2B7aUSqgSj23cBkNGBKXyAPLk105nTz2hzfQpfSelqkjFRzDq5WWd4U0%2F1PfQ3WHFvughHbiZDr3rvbVgZihwawkB33nNcbssjcXimWu2RsUGAqI25Bk0wXPsZkS2uKzHZlRqJFEb%2Fh09Q5NbpaXFcVoA9WyteyZF02vl6LVfxSPz8QEtqjJE%2FADi76xDXyr3tP3q0O2rNOEnW&X-Amz-Signature=9e90b2ffd32d84ccf55142168e7836129c33b0223b28dbcd20312a9ff683e83c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

