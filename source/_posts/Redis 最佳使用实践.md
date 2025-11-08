---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UP4ROHYB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJIMEYCIQDO2PrWiq8cMe7rE5YiUOWU%2FyGejWDhKIuVG3F4DZo6EgIhAKpx6o1Ap4748%2BUkC2hWx1CdQMYOVVP7t2hFdMvijznsKogECNz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwQuA2IqacGAYIx4iQq3AN2xKoOuVH3eIFfBd1l0vre%2FKxa2vPrQTlbPR1ItXEyAZsPFDu11BZBpqKgvIlcHSPZm%2BdSwKOb4UI4I56eCE1Z%2BcuWB460LSzEa8LlNPx6hDF7zF08ZgpCf7QuSUMK8gni%2Fr5BObXKLudw95qzFKQEXQxaN4N9%2Fkf6rDjjjPNmJiJh2uncDcZXJ49bm%2Fq8owm4qz4LT0THACJbs2%2F1JHjDgDAWKUQJd3sfAgCHtTZSH7fFZKWHCfOAmqnRzgOfzSVjOcPkuDtr2Q%2BqbGZzhb8rlj1myQ%2B%2FVJP2POrEmeOuPZd7WSaBzOT1nQWqIfi2MHLgebznyJFWpmAbyFSE99YdMD4adnrnmPIh8sGgZF7dAkny3tKFqycyzwHp1bRvqe7Sp1Ap6I6Hun%2BanMtpb40qIHzFyXiVW9woih9ruT56aMJZbjnhEmacNrPJ28zeRM2aR0yq69YkA7rWN1XsIWWctZu3GJROQTeeYotoSkldMPJ2uE9Ht6VpxjyR0YOuIVASzE0GVudcuOG1LjDAc%2B1feR9MfZ2TmCnPvDhBb0oiH1sbFgIkS0ejQG2aXUMsRjHeMzU2DwS%2Bc0806qSfrQh381Hl6lVPG9ccbWtC38hl9j0bHLre4%2F33niAZDjCSmr7IBjqkAWG1I9LXciKqlFHpt1Q9%2FXapRkXrDGcjnNhx%2F%2B3MegMjZqKBaEcaGQt5yrwL0IZ8h%2FB7z%2BEJEzRm6uOoQDDlzq4t%2FVuVKEWCqwg9LliRwI4CMRCS73ZVa43ob2Xelwk9nPhicy%2Fyl%2F5yNndaEDjhU7xrtMAW6smkS%2BMXA%2F46Vsie8aWm1lUijNtr3M%2BGsW%2FknKcRGNLSMoI716USkX4hyipLBcOt&X-Amz-Signature=7f94c797c9a6c4fa8ad0c91c2c735076e751fcbc80fde2477c59a0b5abbadc10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

