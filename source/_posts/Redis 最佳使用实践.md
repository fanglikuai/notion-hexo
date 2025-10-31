---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VK7ZBI6J%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCIHJhlcgPdMQnUl5wx%2BxOBXCtwKLB5NFp%2BZjuBwxVP52pAiBMgwN3vwGuYE3vCuarepd%2Bfz0r32nEO3URBB2WE2KapCr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMt4U3f1coVO5maE2OKtwDd1iVz59vBnGDenYkaAE8%2F5lP3Ss5Cv4BWX6P6Uucjjgb6YbPxuKLaBBgtueZPPL%2FCH8J%2BB5CgAljBDsnG2so7iXh6BUlb5riSiXd7acLDB6vdPH45SS7iIxl%2Ft1z%2BL%2FYuDbRaf8ireXAIUXPMPI%2BkV5l5PrOBqkJP5A4%2FC3Dw%2FU8cOr4uByjqu1vHu53rhfdAhUW2Ae1d3as2D5rlZgN%2Bs6SdIXmIuqGCAc0REWNymXn4XgDAVKNFyjaQHcicJVZi5xVdDxT0edD6Hz6F%2BsKj3VH3tHGaqhrm9%2BnlThYV84izTr%2BCyE8P1oqd99dMfffyFmUnwkozlWGonXVcVmZ90pFx3hrGJDUTEd7qncPtnVEK46lmiWaohFHNRAjwCb%2BcA%2Fqh%2F8jY2lp48PdwFs%2Bo40dOArpAHgbKKNI6JIa%2FfTSqe1wvd%2FFCszfynTrulAcE7Htlr8yUb5ty2pH2tNw%2BEv9hlV%2BZe8CihKDgwoB%2FYTT%2BfXWMAR%2F1vWj1DKiSCeLVVvVRgQ1JBFJvlTqR10eZqsu6PwHH%2BC4zA3PUDGuVzGLxlD8sxIxGrT2Ux7dFs6XXgw%2BbnVrD91BSGwmuZSCu8%2FvKVaWvRgRtNUfq7JvR0Jh1V8ZLeIKXd0sQwgwobuSyAY6pgFbtXqLCqUyglOkcWd7SuPKgRBjxSRlPlZz4fFzp%2B0bBYhfClWWIFbBSWAy1w7DMUD9K%2Ftq9o3V9M7kmnrMgvwcgum9TxBRDSIMOgZWPe7vs6f6JqnzHQ6DYa0mUrq00V4%2BaPWg7lQM6BAJpgGFLLUdXKtcJRFFHEgtwVxgWXkwEWObwcxzDtUXHw%2FlS7ze1dsJv7cyBDCruwPdngo68fNUaQh9BL0E&X-Amz-Signature=0c0318d0491fa55f37b470c32df94ae4a7186b37f1072700f933a6a386e86245&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

