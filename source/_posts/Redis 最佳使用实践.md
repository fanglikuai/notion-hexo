---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SF26PZXA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH4OL3QL64xRQD4RZCqKZwq4V5jw8SxSvtLdcQhPmIkXAiAla%2Bz2wLcWQxuXYLZKB4Wg%2BtOKM0Jcok69ZQ2LUlxQdSr%2FAwg5EAAaDDYzNzQyMzE4MzgwNSIMOMBHd0txP%2FJAHX%2BcKtwD45owEDE%2B%2Bq5lHpSBZ3p7RCWHFKIqx%2FcH2Koq5xcZyjdEq4n76T9AGE714SBKg1iN3kmwWpntWPvw3YLq5oVdV98yx%2FStOGKaH6htwMCaunnQHl8Eq4La5V42JtazlPzha6jXebf4N9OdRp0tY094NG0JIbG%2Fl%2B5E2do3or%2B%2Fd%2FAX7TcuxVFfULPnCy6wcN9%2Fl2z%2B8zpQ%2F6vNvfwpvdoyhw1DRZF%2BqBWXcS2tbxOBck1BnsH0GiK9m3yOZPxGtCi8JqMqW8ZZeHnZCQCKB66Ddeq3jsQN6SlgNRFd8NRKSd9DE5WJQYCwUX5vcz9XAexUTKs8B56rs%2BULLhgnAPZa77mu57as5NRLk1yuDDCdoK%2F87AKZfSYBQv6BfHZNErMk8XBFGLmxo13D2rpXx7dkZmDMBCT4BKxYGm9LZO0pwOepoZmNa4QKkU4bZUG47wQYVe7AQK3ds4I7R377ujcKn7ISN3N7SYZUcVC0vie2VO71z%2BQRTrQjmaf0NV5euu49qo30fjp73cIV5uC5LY9wbKZytNr5aIiWtRpzsShXjoBpYou6%2BtWopNL1WD%2BUawXNlTUiNI%2BP3U%2Fec4NV5R36WAmtfOG%2Beqe%2B0lNo5gikAG6X3fiLgUEALZLKJREw5trlxwY6pgGGBWDf7IxBjmLic81Oqy7ZLIU8y%2BouYrG0ueg%2Fcjapwsk16aoHVKJUmWUSgk05VxBpy1gJPhKWxA%2F0xaoe4HICxYublamvXqNdNM8sFesh9Kp147AildDlrM6Cwe%2BNhBGb7Qc%2FnDB224RSyFIC8H7yTmfKzGQTQ%2FkLLQ7BGt6HfMoKHdns4dSqwpn%2FelL5MPGGyxVhVgrAg5XbgZbFkimviNnJ7%2FMh&X-Amz-Signature=b5e2c7d2e90cf0fffd4fc18338aca7fd8ef210ff0bc08d34d0b9bef6113626cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

