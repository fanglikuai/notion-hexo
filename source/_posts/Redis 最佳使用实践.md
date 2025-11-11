---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVO2XU6I%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T120044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJGMEQCIDEuLqvVersfKSjWj1vOR%2BZgaxiq2YglH3jc5AWWb2EkAiAbjPEdpPQMWntw6HkZwlflP9UXCBatPgwK1V%2FDIuEf8Cr%2FAwgcEAAaDDYzNzQyMzE4MzgwNSIM6r8xUOEW1Cn28ZLrKtwD9gBp6P41DXUZl0xb%2FMytkP5vCtB%2B0RtIVWs0GhtgGSqiRrdjphQhe%2FkIxgJDM5M7POMOJgOMN61qps5xN9Nr5dLqkQ50zvroN44U5eZk9P8fAOP8GsC2sc78onr99c%2B7rPat9ww2uU4NBYu3FFDgFoGJoq2yprukckvqsyD7YHmxvJdaK5HNmVDDLDZ9xLV4%2BVxjf5Avk4NV9q647RhYtuNjvd2XQfIftkSAMUJsCWjKAIkCGyTrbXfpRLuRWqc4A9J1YooJ%2BSVD24Qex4Q1OuCPITkmig9gRVfaYLL68ZHyo9RCLVIggDRcce6VjOd8GBXbPXGcy0wA7%2FEBOLEDWjcshYArr79ImaBfydDnEMZlL0X7Ncr5gRgRZWHa740iFoYPpTPYfXuA1cXbQ2VXE31zW2bcip2zNFsmU5CUyqll%2FCt1MdX1NM6sHu4qRhTnG755FDODQP0BGKBGAvI1d644xwB6BiwIox9gpHvd7NKAxPzfa8ukBDoZtD3dG3fhF87qwe9ih8RDRqtMp0CCVmHACx4jrzoKAHQljClUmDvUb1qtmh2yFoXgaQLi8GBvEVY4RXOJyzbEeb2zcjTHpTcQMPSxA92Vp%2BDhRaYQ9MwKNZrUqPreAjN%2BmbMw67bMyAY6pgFQInW3U%2FdyIPvzwnaijOSaGzofOALIIo5fB9CpNOCVjL05sns0nJgX%2FpWVMWDvlsZB%2FQZ%2Bto1bxCnhUPv5T4lqSTm7ebc2XBTaEVP22AvOHpahvjm93rLuHP0Bvi7vQ6zZ8kZRPN1ixXiKjyzHVLnfPNSM9Vtxh89Wx2ueMSwLnor6E%2BLrq1M6ZfJSD3C%2BnkGM5k9f7QUVRzmH57WN0Rgg0IiWqyyD&X-Amz-Signature=139f2df47fe70c3fee0f0f007d6945de8a8be379809edd7938ece0ab10323e36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

