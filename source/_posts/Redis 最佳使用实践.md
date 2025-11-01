---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466672LNUUU%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T210036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQCb%2BlffLmIX22TsAHsadPEZzDc3EiIu0uB%2BQ0E0TIy4DgIhANrZlKcsnGuD38vEh3wiCL1eChjiSFWDAU7wsY0zPCNqKv8DCDUQABoMNjM3NDIzMTgzODA1IgxwAzmaOoGZiZBZWbcq3APOThzxtIBrrS4GX90Tqfmh9HmLgvKUVtPdF3MaE%2FZ4OVyIO8pyzXaim1IX1nX5yW3ZcQhZNt2tT0sY4wFLNzfaGNSZqbKcN4v4Bd%2B1tySH8NAy1pfyhFaXVqVyX1f0eUKL%2FxbP7%2B6ouEbRmAkbIb17Trqw7MowVYccfwoGhQJYuOQxYV4mQmIMLD6V%2FNJMLWPp2GpgmmTaLm8ViVXSSRfoLyMJOjVhkXHDVhNDL9Ynr71K4gvbUknrlJDkPTHjlSGmuQ1mMImXJjQNe7c3BOHkAoVN65pGx4FksU2gpH5RAQV3onuISHct%2BTzSHlsLijPO8oMUTGm74LzYo2NiIvPX820X1EkoOfZwUCzqiIvZZpfE6nm1ixLktQ6FJe5WelrHjB3mVaw2xFa%2F1XDTPK6po6cpRCIPlj7027IBYfL49Al46Wp8D2p1qdMdl0Izwa1G9xZfDC9nqxKQ1JrNywvsdUnm%2Fpmg9y6OcFCJ54JHd%2B81j738gHEtQ0MMocf5wIiXmnUdjOCEXEtDzT3DonZ2vsrEbtmqIfCd1TybJLDElukYBDoafpVRPt%2BxtRlxskPz1fJVy1acxrpmD0Me8vwl1t6SqahQNsVopC7puXttvtBNE1%2BgeOsnndljKzCiw5nIBjqkAR2leRqARulTwk42pwEQ76BaLkwSwc484OxYEharFRhjF3vOtDfcBKQcVkzhd4O1ADDv7Ihl1BVIihoUKKq17cbt%2BUak%2FjTOXPKyhWj1UCfXVvVq9ZxBOaC0%2FnZtmC16t4MVm5%2Fyq31F5CEuYuX3qbpqVNP5x5q2DVgeA9Qrhts5Ueu0t2KHc%2FYIBu9eYR2sdne3rGgSp4%2F3TccifWcQHe%2FBKSjl&X-Amz-Signature=5153d15857f204663ff9d5fc2984b4bc0556ed1494120c65093721560bed0821&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

