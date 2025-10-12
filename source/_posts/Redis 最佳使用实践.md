---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQFFFJVV%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJIMEYCIQDjFxSmBjL73KQULAr%2FhH1J6qPIDCSF%2BZAIVhP5icDGVgIhAIbNdSz3kpM9zoyWUKjayUua3ZKevOg3lXyPVtsWK9CGKv8DCCUQABoMNjM3NDIzMTgzODA1IgzeeohhzQNTC8VfE8Uq3ANx%2B21PGjWxHIDvBZ4gmvcrCQCFtSE50d2eXlhIS%2F403T8BbK1xxvb44Ao1soJbKC%2B5s2fe89L03rJH6vAAm9xe3hEhwIchrB%2FbtHtMd25wWFOwYf8T9oMdD7%2FSWqifkiYYvXn5AnNxBz6mg0NnLU905qBXN3m8nV3Kb26HNFtzjUb8lLNUFSAoCAixPfmIMkzIGghk2CtM%2FDGZlvdWVKf4HkBXxp6Gi6urxCH6D01e7Sdnp7rmwelbqSPXHV%2F3emGIji71dp2Qzj%2FsnWYYTyR9pcVc2yO%2Ff0yffvgIiyaCX5d%2Bl%2BULM60QXvwFlWgjv60Cn9vUpUCzBMKyoD2zvvL0wAjUjkzE%2Fu9d8d7OYR14awa2KQr4AIyZ0LBY6RuwdR%2FR60uq4sPSmeMXsvjvqWoINI%2FCmkHo8yA8czb%2FJp4GNLuM5IfnCj5afUuVNzDs7T9vUvCz5mKFAHhVeEJcSQIyNsmMoeHSt7kfHFV0PxAaWYxx11411VqTTVLCtHaMhYuE8g8pmicOFRnEvBfEWzF7PFIAbKtcA3XAuY8aYy9DFIa7NAWmMgD5s261sKGKmQAU3JbTmzgtqQ8nB5u45WLcFkYMR3SBjMwgyiy9l8r0NsmXIyL%2BLoqhWXSNpTCZzazHBjqkAeSZV1LJzDas1kdnG8QKMJZS9r%2FJXl88TLnyTixp4%2FmqE3ZeemwWaUGsx7JtisiipP2irBEFMy1dY0%2BC%2Fibnj8PuHgut88en%2FcIqNoIMBggtiFNb66ZpqIt0EDMQMG%2BUNMpbnQ4FYI0%2BmyDNf2u5vnIXBAR4n5HMr6htePmo%2BmVTAZpPvS4BmbBfmSGxXcrVSuGURvErO4f7kr0ITg2tCzgQMzqZ&X-Amz-Signature=f2ec934136bf41b4e4c1ccc7f774ab2bd525262d4efc4baf0d46544c57d64537&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

