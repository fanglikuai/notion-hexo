---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JDGUL3I%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T180159Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIB54f2Vd5oKIXXENN3SqTmnZxAeGzcMcfSTVCiuK%2FHYWAiEA7zXCaOoWnr4oYJN6eKx3qOkykz5N6JW7eAsabfj9ivwqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIGapU3aO1nSBqMF5CrcA1WAk0mVn6DkvEu%2B4RjvRq0AUR3hOJQZOnZ12xqr1ZFt2QLWRrHmktHPDZ1OqLcU%2BKTBOaMmWSN1yNLz3fTmGH2ztPiBr2LTxwoC7WtdPih3gHEcY%2FkuX3fhKn6ZK%2FC07%2FBsNWXws5fdflZW9l7WXZnf3L3Ecs7LpgVD6VLJ%2FB6oybm0vf20xaD%2FaAmsg5yU90RC5HhkhCD0DnS%2BfaPscYzC2KQ6jI0owIRzEpDc9GMQSZaxxKGe359cZq4h2yd%2Fq0WoSMdqpyO7K10B6CWtxicbw5WkAItll%2B1fs6lgFBuAdICadzt99miWtdEjBWBRmplvp1%2FwOexCF0%2F7J3OXE%2BZnzA%2FsqpEtF%2FPEXCVFm9m83EwEYTHCPJ1dH4VQ5qsUzEfuDLl%2FFlnw1CJkQbwLCkH1FC30pqkYM30lGMUHNNZOppg8kxCxsman6xAVsnbYqtSv1VH%2FP9RkLuu24U9Zf2266vLlC%2BO%2BbFoTRnWedaGLW4BFrCUUJe4x6w5hMnDEJwaS2vtqIWtrnczjEW%2FkNVhiafvJQnRcFNHy5cr4B1ygXJoZYDRe%2FLy4jnGYqDBoKfnTOjOd0KevDJvgWadWvWqNSux33K0qdhIbSoK2wE8%2Frp8fMg01qM6pEAxiMI3in8cGOqUB7h2kR8Sk4x8d4GiAKPN0hqv2uyJB0Kbb05ks2STuUjNIW1Z8BLcPoxFHgw735BVQLmmcWFL3wIibe%2Fh2ze8lEe%2FkBskFAx4LjwKum0hXtd0WMqBJoA%2BOKLOk6n95JRvULbFyJ1rrQDVOHYyxXxzEBqZVqeqrWcaYb4Bkm5wURJxujocoPA4epp%2B7RgEyPq9YhNlVnyB2ct31whZU4cIk46VCZPgQ&X-Amz-Signature=fae29ad92d505120142f5d22be88c429b010bcf11d48a5a50ecdcf709e4d99cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

