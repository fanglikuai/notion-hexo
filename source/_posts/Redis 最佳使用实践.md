---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWNSE2LE%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T010057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIQDK9myl36veaAW2X7Buw4fmZ65%2Fa2EeEpd30VnF9KZ5BgIgMTxRgG86SLeil2VFWU80fp502P%2F50HCU0Mzx9bS9KtgqiAQI%2Bv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNK6kB%2FdmJebxub8GCrcA%2FldPO9AFIMwY9k9OE7JJVnZ7K%2BBrSjsxkPFY%2FUmmP6KOl8%2FEUQ6%2F2%2FS5oBVqv4cGiiLjGomXE0z0yHXK3yAijS%2F4020B%2FopNlMhCrN7cN%2BH5W3XxI1QaL0kgcSSysZgzGw13dKzJByok8dKDw1LTlCmT65NIW1BaBKqZt3nMV90YuDGJwcp7tQ1GVSS8O74d%2F%2FSW28oBH2usX%2FlT539H6Ve87t2jkYwLtng9ieGqdXg7ezMzPZmNdH1UA1GqUiGx2YXIkpIkEzJLPSdcrYL5V8avE4q8E0JdYAaduTk%2BpbK1jlaUjTp1uM%2F0IfJ7u2sdeGRjR4Ad40Kbix8IxYF9Hr4k7qlLqtF1Lg6ovB985S%2B6eBoogvB34561%2BBmqVXrta3lS3cgunyQfv10yObRo%2B9n%2B%2FbXeauNai8QsynjfbQVNLKivwVJx3Lf7urhljk110K2%2BoaBw5xrAfzlfLbZiGrI5DbZYPxFpKFspd4KWbG%2BPn3WxM7p3qZrJcAHRip%2BudZE5BEspkbKBimJe%2BEIA4BUR5xQ%2FAbAwPEgZUzVeWlqR46M71%2FrVVZvXcBq86ItmpBe6hew%2B4WCpXwrwVvL2H9n%2BV%2B9ka7Yyrk4NNowKWatbiVbdZqXnaSjsZaUMLnvxMgGOqUBzY81LJhp%2Bo0XDhCgXJEolfIRjv6yu%2BpNuAsc5Nb88iStNRfjuWZHnVvBgJXxadbHgo9U8C5qg42Wm8AzM6geUY5RF7l3jNb5HJIfAFaQ8KUoDr8OSqfStsQhPasjhWXa2f2Z%2FD06sYvD8bieYjZaAwsIoJIs%2B6smizkOEcER8fBf26gL%2Bs0%2FCRWGFbHYaNsiJeC4ip4WRUPNcpMXPc%2BpkT3r8epH&X-Amz-Signature=f48a99ea4d51e376b413eedb88dd8961b81d96927e77a772074602af1ba31b4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

