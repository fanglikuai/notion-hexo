---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PLQZFZ6%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkpprREjZgcT%2BIMWFKaHTw2HZ70cBRMmpXO7ykwQ%2BJfgIhAO5NVTcIxkJqwimjfF695Vlgsr0Ucg6u0LuI%2Fad%2FxBa3KogECKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxAWB0Gc%2Bs61VUCR%2BYq3ANIGLqp%2Bf0bMKwRzUlxZHIa9tPx4jpDVCb%2FXeoS7W0r39HBKX2VxtV9Ahffd2n31MBzMkcOd6zU1%2FKAn4KSBApqUQ%2FXJhzDBBqxJoSXI925sQ0GFRQ1wqOJVrHeVNSEdCCa9qeJNg5oDsNfHOH%2BZqt8mASXlLkjitJSPlfPjIpL8MyD7494xMQx1d7xEfM3bj0sBudVI%2B6%2BEfiafhp79N%2ByChMlC6Gpn1tDggyN8Dd1h3KelWkhR6x36dCaRuAINkq8OBRoYfDZLhWvrDCqn%2B9PrRjBISufLU%2BagmeNsBNjsvTqzDzlJdjSkGOvrtZ3Pj9vYlIiau1Gxw5ZjwdTbAsyU%2FEs%2FxRwHYcUkwVWh1y359BL53Sfr2jrxiYV0SabFELIcBaE1g00zIxk7VsHEdoPUs3X%2B0cNAyWvPo9M1UK46telXz0qu1KJZc1OUzfP0FGOCYdNNdbXiSn8nmPgOM6OZrro%2B%2FThZzH%2BFPLDSVPSpKFZvwi3H8d3ywkR4eFzdqjBdOWn3g%2FNwGTPNcHXTxVv5klX4WiSuLD%2FOIyKEUoAKlkh8Ozo5L31MFr4dFQv30qHIT%2BbcsdyftbWfrKSA8gq0mtP%2FEBx3UXMxlGBAkbrjNSLaGpra0ofPCjgGjCpw7PIBjqkASD7F1c54Xze3ja9xoSsRduTkEHYklrM36oi%2Bw277sSWo%2Far2qmnOi9myxoHOs6Wubo6gncQbr00XoEe9u9qSqeuJ4li5lhw%2FnjYvNvVNiiu3OF%2Bcs%2F3bMm3CHkUI6GE7nEUpqEeAh9P1ybnVUY6kfojXPe9Tc2sR549rIRl5v7dMaLgaEZr%2FZZkgtJbJFI%2FXHRFYVCU5lA%2BxlvgR7J2uaQkaTXX&X-Amz-Signature=7bf2dec9653cc6a64874e7de4d5c70dd144710c9bc3a88740054672b82c0f14b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

