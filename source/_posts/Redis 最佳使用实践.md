---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CBMTVW3%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T130054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDv329Ddn6vaJQ%2Fxq4yti9v8WWlQ7L5t1H4HkwvMfl%2F4AiAcSmdPwGNdixSkWERaAtJDr3RfGEzVN5pJKYTHT20PhSqIBAiO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDZtsPwIwCt6nOOJSKtwDxNZJU0P3VUsjS8Oz0HBM5MyAFaFu87qoEFhm35WWXGWSiwlAnRV6fn5v8K225r5d1qObcmCfpt%2B3xMLv7Fu4pOvBlCh0qc7FFpW4f3JAXRi%2F81qnrjUQVRe8xRfBYA8NkqvU0FaIJgUK7CrG%2FF8aM%2BYURBOGxwcv6hJQLdSEVavU2cviJhSEVaDAimYLlLE1WoW3zPQ3ULJMyP9GTsXoJotxOVwDBjlxqYq69wo8Zz3VPa%2Fm5sCil9XWffSIPXNujjcb3syDjoU4SGV9cljZF9hYaYDTfUSASiDMnOnWTA2gx6iCZLQPH%2Fa0ND9QJwPfZjrJcN99w%2FMX9hHtuR2pw7jjnAG5lDOLnLfFFfhJ1IuM8GjMrOp0x%2F5Vsu5kOTpuT6F5HtKoHRKJx4vKLaxcqmswzxtmlLyufWW70wwbZlRrOy38wI0Y2Ae27octnZ8AOKhYPQPRvG9o%2FytiwPZoqlN5bqiAHhKIN%2BD10f1JSyqp0bRFaMQCjTrY%2BMbUUYRiIXS4ZZzmX94SAIq8bClfVshWvkk%2BwrRaXBM0csIYIOs7x7gBb6AUGs9vmni1WnvPKQwXABPicAxF0M5kl8ZmqfnrV2flxcHSFtvqLywp7FeWiIxvn8%2BurZcJCxEwpM%2FDxwY6pgEK9ym4pTETf2GQ3OqbnWeOE74Vky367SY3jZvm2zYDeWCm9DrFldlryNaVaMP3kZNWJ4mnt6%2B2L2HFRFlUtnaI6X1X8abawq1OUZhmeYhds95AW3mUU2LCFn6hYGelmV2JZl34vHRfFZ5CyHe3OmWgr4rx8O9qTUuRV0IfwcnXdkjCQHb6YVqEm4sz55Fa5EaCM1W7kEN3iC6XMiYrd%2B6Z1LH9M8fL&X-Amz-Signature=b4754c84ca134e47edad5530c0e7c7972554674af3ef17266f8c5b8793a3b2f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

