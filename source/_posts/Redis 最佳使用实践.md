---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTZEANCU%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKHel0Np7vx91I7eZYZiKqDaj%2BqMX3HKZ4hT9aBEL9%2FAIgAfLpGoxveRl8o%2FWoufqiGe%2FSwh3iiqhbwerlJRqme18q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDAPbfGWy7pKlldau7SrcA9Z6bPDXnin1PzcgqK2KDR%2FecT1mmfp9GxhGf14skV2twgWzjsrh8JJXaNThHd1WFdG15frGOVJOYdRzk%2FHRKa2DPiDUlK6zUjsjYBTtFVxGJBtNOkdx9TdSj%2BGc91ZG7db425QfYASdLMRqk3TWCHfnv8GUPm6BdsO76vt1EOhzEhzXLZ8edZTuNJheM0lqfnQqXnzUwmQlBRqP44fUZfF374B%2B1u4rKafZTH5LZXezYIX6IhFavrXVKyYkRqs7S%2ByBVSAzu7f2sNrZ%2BVaVBAvGLSF1B1WUJYHadz6G%2Bv2i0FPRplcUm6aT4JCR0GG9hr0Xf1IVvAqwlA7luQHiOo%2F2tSlaAcz69B2hnoa7Wmr9O2gP%2FwksNsG0rMhktAoiIk2c6At3HNP0wlEnfODqqInm%2BiViMGvTVsiKWixVGMC82TBZ19fSj3JUIypcpW2ubs2Mi9uHVAgVock1QVJd6xYBeidbmmgq8CYlOqdPbW7ca%2FuqbSVr20tg3nN6XcopdDe%2Fnt1bqpHZm25akXcHDAokpr1fKn%2BSyGjkGxGcdlGvCs97VxYpNUfquz8CaiqOswRSNAPJv6VXOOGNu3%2Fc1ZbREIOOj4eP3MUCKG%2FPG5x9v6GYp6kSEuQN3MWUMMDpwMYGOqUBqgEWp%2BGcepU%2BfsOW0WMFkIZBwJpCvKTVKWvS%2BztoYGP%2BddjZCrVC3t1lISpSx4XfkMdROId%2F%2Bma8aL7lqgNc70chnmntAs2wOMSSM0YYGWP2C29lZ2mMpNRWaoQPWnaHUQ3IQd2l7eggQ98q4FxV4TtGCEp8MfMT18u7HXRsvoWiVK8rYxqKPk4e6FluyjwrprAOiVAJgobhk6WS6SkySYhB1AWg&X-Amz-Signature=90243ffabf9a6b9cfb67c56b278f3c4bdf5290eff9ae44641912165f3be92311&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

