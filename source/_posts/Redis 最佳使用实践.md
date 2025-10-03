---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JBWM7YR%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T000038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCPi%2FAlz25wBOmqyBl79RubGb%2FouSZ3xq43eS5bNtvPKgIgA%2BdXgB90dQmNTgBDpODjZ4jV0izaU4VEcoMkbJx1m9Mq%2FwMINhAAGgw2Mzc0MjMxODM4MDUiDHPag4LjLxE2XthT9yrcA02Z6SoEplcbFzrdhYRuAKqn6MoqkO1t1QGWNEjj97DUtJhUMhQ4OU%2Bi8whQ8k65PyP%2FvalPLUpYXnbDf5ndNup3FG57i%2FkOwUxHhVow9bQVep7uA6%2FwBdlk047AF4hgQ5GuZ4iZ7JMu74W8Co1wxLBemwYFTqanc1ABtAuLfYgM0E8KTM5Ujc7U30RfQQMYiU7GYwk8zOkA%2BvLDHGTgVH5t7rp60HK1qUTu9ZT%2F0WEtFf3ijR1Ra8VlJws8fevr4GurUNhO%2BKYxp5cZsxrjhF6s%2BB0DMXaI8YYTB34JzIy4J0Wi2ghfw0RGwVBIAYeW0JDML8gFY43%2Fb9x2RCL5%2Bc3iP6bVg0c7AiiWtxC51E9I64c%2FHcyfN%2FAF%2BUvuVjsgZg1kjtU%2F5jp1ChbKvKG151ngmctGxpdKt9fBpOzcFsSkW83Ybppxyqt44vlKhvtXDM0vyn2Aleb5UhqBecnNmjcq1s5FUpy4mdOA%2Fck6YzwOFX4Jww9beRur62%2BmIx92RTq4qa%2Bhxoh%2Fp%2BwyEnnvt1sUJ2geumKBd%2Bibk%2FspOes%2F6A6QImC%2BXbezE0UFFFRKLQu98RZlffWIKUTvtZdMrgrEjm2jGsrcfWAxT0Q2YV8OtZ%2BwibQ0y64gjv9MMMna%2B8YGOqUBwtEs%2BCRWb524SxRWZVIesqqut0evZROAfxSlZH%2Bkfugw9763A1N43AKCgPGgKakZ10L2cKWGocZTPaejbQQ2xVoWGKUd2mc3Ynspd7n1Rk048JDUkWmyyhQBHsLSSx47GrlGwdWRFT2yF8AIVbwPJsfwp%2F4zGxwSwlOSy6lSHQIxt7cc3Knm%2BSjxzjyGE0thXU%2BtOK4%2BeXOewUl7YgN9IUhVr5AN&X-Amz-Signature=221d7ac913a1a118c65923226950726486e4241e8edcd0386684ecfb7666eed6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

