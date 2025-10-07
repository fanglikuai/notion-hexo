---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663LTF74DR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJGMEQCIHDv3Z7CapZK0%2BYKqROdOvrfztqTNGTNBIVBE11%2BXvQzAiAwwleujgFvn43bcEPiWl7gtgJ%2BONsvy5TX33Jb%2F6VUByqIBAim%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcxDttIf5REqIe6MAKtwDWd2Mb7TJj%2F3osAlxZSLRDQI%2BJO4RK4H1qVqpR36rIDwMrCspX0pn%2FhmvvHtUa3CFQxibj%2FOOsotaHXR291bWA5RDncWkJp%2B4Ahm81rL2OXT2wFa8cjbUy5Zpa94PNkByv4KEj9UBE66cejIhRwla28LgM71BtLSFnKLN6JUAVByQd2AFCJntzcUv37O%2F0Zudpm4YCJ5pAtpkN0Auts81NrMS%2Bbxpek%2Bul1rwRPpm1BV7Xp8A42BI3iHm%2BWRE2XhE8h56CAh1TzEc3f64YsnVOlwyqqbzvPYSRXBSJK7mfx1%2FKQ44x8YqFI7c0ILIjwrnQXp37dMyMploPG3XKWWULR%2Fe0wvnrdLxzREdiShpwIPXpCfFhPERIz%2BL6v7KSorLE3t8baqVgIuZ4xTnIBiVp7zVuhBDiQqsrDkHmrbQDrr7nOQ8HK0PEXTUSOjJ0FCkeBPtw4kHehXCRqkON2GgFUb%2FzmKtMtd%2B%2BNRHeliuTolNwgwpifGJJEsaS6REGuAcRwBUGg6JaKgGyMIliKKzkiMvQDD4WStNEPSVWqt6jL5fPFZQiy8lzPvYN3%2B%2BMnVwDXbMiSA87y%2FrPs0j7XGOtP7EDPCaOdqoog2QSVhdDVzQeHLcNcDrtVO8JogwqpuUxwY6pgF5u8hXLeau2liy4clenWLf%2B05YDksHqz6iCW6az8tqtJYlGNssutf9RepT%2Fz4W%2BmZxzLUm26IROdD%2BH1wYSMCtcM8hXS5aSVXjYjbQLok3HXZhtwdZOsU8Y9AVlGN0ygS3zKumEOUNE%2BrvWWMV%2FKMrXM6CVKCYCmU7hkg7E7ysg720fgu66f%2FLhjJRb60VD0iWheBIWiVknJrRK0grGZwiQ2HELxlZ&X-Amz-Signature=3f8671459c1577ca9e989102f125ed0e2bc4b43b657460f2594e44aef087cc28&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

