---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JBMEVOY%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAKmZwEBHTBaiZO%2Fs3tGmcP4IfNqJY1OQh5mn%2Bl1oHweAiA50QDpHPknDvVovbu1DAUi9MjKMHEfmM3AJ7xc4yuB%2BCr%2FAwh3EAAaDDYzNzQyMzE4MzgwNSIMNRKgj3poxFXH%2BDHJKtwDGq1AnMtLNH%2BrYhun2%2Fq%2BAX2UATbSFp%2Bq1hUY%2B0kP3miZs50XIRkhnoAO%2FU2VnO1YPYL%2FgkV9cskU4JFjIwL73tCXTltxQQFP1oKiTzAdPAVVr%2BsCZs6k5s%2FJyKsX6tvwZPA4KSZXJBQZY1CoGZ8ZR7neaW8hYj7TrV96d4AkQRUcFSE36y5MJlS15SxOTbS0xLP0JjQuoGm4n0Vu%2Fg7gNHkRXjAbmXNJWoO0NT5JO52u6h1b7EQ9NbSPj0gFOV%2F2ZmP9BnXijr48HWegHzlHD1uW38j9mqQWNMGYoRnI9kw3Ja7746qfVzUqS%2B07EJqeF%2F4AteWrrqMQ2%2BHNTsH%2FdMDO6OQMQp2r4AXtcs9nRa0xL7HTDhWyosNxseZvSG807DWOMjF2zbeti6Nreizc1CC3emSpk6uJCDnOHkDJ5w5J1nljAuQqqoz89oZSVT1IrBiNchP7eYKrPpmA0z3%2B0GB%2BP75Vt2khGbzLKRnYoS0ZEAnmJV1FxasLtdnBoWMGnXgyD9SIAMNKhtfjwlIednKzpM5csyljKwhMfi6RnPWmxcRnU1WuZZ8%2BYTNnt%2BaB75Kz7HGjf5JcJXpsCAg6QKDJk3SFunjle1bPtwpxxEhGm75%2F6V33LH3nXOAwwJWoyAY6pgHtOE6s5XbI1p%2BcphQfFH4lh855oszNlEAxN9lnL5KtpPKfC0VNDLho6pe97s0FQQ8DWu%2FklHK8k2SLF5%2BguJzA1XWv94rz4To4vjqGQySdc3bK%2B1arZcD%2FFBVj8LiqkmNs%2FGebNfJQrxSI%2F6i9MnqJwB4mHDDzWcxWv9pCFVYjxuAmQPyTXWcyd68Sk8qi%2FL9Jyb5gIoDM9eGE8zJKdE4glJodfV0O&X-Amz-Signature=2ada799f662de476dcdbeb1371184eb00339956e001219c13491fb2462dcb2b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

