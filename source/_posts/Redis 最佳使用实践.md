---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4F2WGDS%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDyeawybrtxFv9Xrrga1yUc6yZbF7TTmqf6ITFQO35IIwIgYKqhEwtETAsAgTjvaGEPPOWaZ4OwPsut%2BCjvVLOVc10q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDMBoGnA7e9La0MiR3ircAzi5VvL%2FShtM3j%2BOXg7VM%2Fr32WDXy0LXFkjIE3WWg0KrleqJ0cZ23YIyFjTIhomn3ZtMDS5eZv%2FvmKC9l0734Ynx%2FOBWmbp5pDYuLxFk0idbIiRYlK8QZMJjUf87XQHsYJEF24YntWtetlW8N1J16QVkfHaRUNpIOXP9YWxQgJgwXf24UVtWhaU9qvMFS2Fc53y9O0JmL6lyzKrD1x8fsRxRzLUJgxqd8zTdv9HPiplGmUkv3rfr3g7XtOsK8Dm%2BjHtApQLgHVw8vrySL8cka1QCeUv5SJfM%2Bmn2bODJT2CS7EgAFJuj%2FOm5sj6OSMxHQOcxuEFhnlKErLCuz3dbVHgCn1b1ujoZrnnqhIFN%2FIlFFjmKC2RAZmyOJuCbEVAva4Jsx9v0NcBpoaLw%2BPzKGUvO%2BRTY7mW2CoU2jfZMUTfMVTTfa%2F52vWm9IlGrLPegUt4Nupzn2%2BoG2WaYE9QvmVdjy0X5vvujXiQqAAE0he3A%2BXznyi94HGH7UAZigN0kYVk%2Bpt0s30XdJtnMtOeVYLdkeZvIBgdpmVE1vr2jpmZUY0dm%2FBUEi3181ZxnkMzdCvdQI5EUa%2BpiJ72X6GtLen8zLXSf8T79U1xymi3VlcyCBVkLhQHxWKepng3lMMamq8cGOqUBY00%2FDUSuLp9NYaw4Sms2Zt3Nqf41uazEenOmNkHg4s1NTFaUW1lBDTeV1PQrObWMjInED0g1JONQR9v%2FoEQeM7DvFexdB9GvbE43OVBo1p5Vc34i%2F2k1aOPTY3x4z0VY7UtDzwCPxyuTw9hw5RM5V6yOapK5ITz9UjKvscnevwYDWSQjvvdIFkBe%2BArWl3Uveiy%2ButYS998umjcwGr9hrsc6zEmZ&X-Amz-Signature=8ae3c6809746698cbe6adbc91993b489e3cb313d80d0eb808c17eec1bb5d6a48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

