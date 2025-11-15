---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ABEJYG7%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T130050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICaS6Bxq9Hs5LFCVBMz6SMGOhDbijVmxCQrWUF%2FmE8ssAiBelNKqbAWXmETSPGM9Q8KnX0fiL0vQakwc1eyYqbkGCCr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMQBPeyy9FKnwnP7%2BHKtwD8hj42BiTATMwAckDWuztUhFp4RVrkEErR%2BQ8avd1ojjCOMrJDQAGtJiCEhQRjargRjD3RP5U0kiRSGT25Hx87V89MkwR017Ra02A0CSCh5h5CgeUcTVCpANlI2evE%2FBYtO6jCreqFbg30XqWD3wWunkFmPBEy8Q%2Bhv1H8NZIJuRAT6LoKJD2cskQdP8Hg2ebGGRETT5S7a%2BKNpHKI00nofA3r1yd%2BiLB%2BkVpkqwXI%2Fn8M6FtDum9F5YnmwwCoZUQnRRyE%2BoudwWPWz7rzinIwDfOD3swWxig%2BpKBGNg4bLiPaA4KoTjozL06M%2F7ZYx03Ics8MQGYf0s3ppahHYfcC0n6WI8zeJNkbnOi3MyAscg6uRYpCS4GbP8nFI6NTKBXCoG1pt%2Fl8bByfMdKHEkA%2BdymCYYHHQGvUm683jRZpnCxlRUzlIAB80AKO200Fe1cuLjMToa742mG7pMAZrEzPj09gpOZCzBCWb7Cz%2FLrxFIgUrsfiCbX02cgIQqbAER66O3dog10IXR9%2Buep9l0YkKXeCgrvjNXjBpS%2F5dJEP%2BJfjepz0AZWSzClC4que1sFisAotcEBc5A%2FRl0cLeMcHuRVnnAMsU3um%2FtI%2B776UNw3gA3Ze8iNp3%2B6FBwwmYPhyAY6pgGf72n22xfZvH3xn0L9Hapyk2rsXNM2LNdwCdNdNg2MBvzC6XhuaDS6S7eVoqwyjBuu5KvY2rN4PtlB6UTMlBDuLefkZZSOl%2BuOorIyw8Cw9jD3GzaVPIMmpZJa605tiK5E7jlgZV6HpbgKUg4TGEDyqBSOdV9%2Byl772ESqyQxnnx5dYx92QNoShDANftPHVM8R%2BMyA19vyXewjmNr9TG6P%2BfJqKlYB&X-Amz-Signature=88a9b6874c6c8e5ffb4ca006a54ff81cd6ff3de4f0b861f9e5ceabd9f88cd01a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

