---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666X2IJH7U%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIArbjS1RJv7O8bSL7WqYWgoi7PxapY7Hlkv9SAg35PEdAiEAgm8fI%2FOuwC6RcIRw9jAhs3VXu4Q5BDhQGXfWEsWsnbkq%2FwMIVBAAGgw2Mzc0MjMxODM4MDUiDA46e%2FMcqW9GZ%2FCaLircA5PoQVn8JyXIHZev9bjW8D6K63lULLorOrBubxx90R4HHyuEwzLtXgmHNNPXROsRYovcJLe87OO67aT0xes%2BmZHOzt1KDPiyLKe0UJK3PtfDoqCFsh%2BdcfOwhU%2B00eYqOf1p%2BSVmwOuZ8f5AumGK65oJNnuIDhOtSIm1boxKLJ%2BI1JBa6REemZu2KqJElIc4JA2CGtc%2FHVwHmrAx7xUy8r8ih5UQhT%2FfNb0WWYuZqsclkwYIzXmYxSVlGxCaur9NSmMK%2Bpgw1%2FYh%2FuVaMmzJKnKPtF8jxwNIwAXXGgSD%2F2JF%2BM6MyhZVYE0QCGcNs8V8EsEGHHG0QqIsIEGAdto1sB%2BkMVxRLAoTJ02CIMCc8vjmy%2B0OOIooSSAkbOiYKBe1nqFQgre4d9cSq9tzLDnJ2o2EXmmnlabYFr2zlZNVTlNTydrTgTid0kVNyIZoD2hpGOrGvmeZE8tObql5iWGeJNWZTPf2ZrTRtY66WBJGIq4hukBbL46KLQA%2Fpbj4jfAPlPYJ%2FHUBiE3vG2kpEetvvoqjjeqvByg9C8a5etnT%2BSizfphG2ob6vE9PASHSHl6lLga8h812675iygydMo%2Bu1VmCReDmb98IeHSTElpBLkXp28C%2Fn%2BAYnqqbEpCKMODQ2MgGOqUBYNk3d3NQxkOYEPAmDMRR5wSNCy21ahLlnMMR3vXl%2F5CNGr0XtS2SoPF6J04P0pXUtZ3wjICOdBvgetxh2o%2BtaO5Rg3jFacsW461WC%2BLTojV6L0EuPzlf0m4O4TX8gLX1M5lqGoMuTbEsaZYSvY%2BqJ%2FHxNW%2FJb%2B8ofmbWB28eQ6omKUF6R%2Byo98AKpIooBq%2BAWuO6%2Bi%2BZxs6mxfd%2B4nQsJGhF3QoT&X-Amz-Signature=6b2619444f00134c98b2e9873cd2e01d635932d61172f5691d4228d3c6765379&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

