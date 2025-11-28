---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCHRUXDO%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCtVT0uRz8wyAZ7hwnFS1ig27Y11BKMWPskEBWgu53C5gIhAK7EEoxwACM3LFXx4PUzb6gbLjRhkt89jwavJTJVrP8uKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy1%2FH1hc6LoGqqaFsUq3APIzP7k9SGmbwwJy6O5rPUfZn1lU8T%2FExZmmVg%2Ft4Ni5W3AVjixKjAJZXeqR0839xBmqBEBto3VjioRnV%2B13b4wxZrHFcPLFXyA12EK6%2Bf5U9rd6a0S0Ii%2F8lMdYYib6%2FOXAdXWvU3xMbFk7yILEML8psa8fnrDb%2B%2FRgpFLdJLIiUHRbtgH95DtHhVZpQnkAWGd9C4ysV%2FCM1m7poT9lVfZsyM8R6OSEJq%2BcStgBAgl5zJIaqUtv5l4LGHejg3gMi0o9xLo4q5bjGKY8Bbwp8AHQvoagprWSATGqoy5%2BViadrJIhqZTclurnyn4ll3ZDJ%2BXRnq8K4VqunPWBd6ORmIImSR7LZsigdQtMjhI6hbj8zLuVKcfW4gQw9M8ycZv0tzqUm%2FWAvrExhhQN%2F6Eoa98NwzE%2BUSwX4PcDrLDRRP%2BaTgZHnBxsUkUywOtmP%2BYpIWtjjL96NTIL1tBNzTn8%2FgxBKuVkggtbDkvIvnmdaOsJbEPJ38jdaukKpk1lqwki9oDpl5iH96ZNInpfudvAzPxaUhVoa28d8gwazwPamdAH7%2BnLnUZu6qx%2BOzUd2dvqf%2B%2FZWjtbfuxUHWtCvYi9kwlXoVkjdWRqXuEhOq%2FCkK%2Ff3elyRwgMmVWFC%2B7ezCswaLJBjqkAa5rhqhppyK3vsJD4%2FO1DAgOd%2B6d01Dbe7OjUcCpiK818InCzDFNb1kT1oTHe0m%2F6kMXwjeI0BdLmTBZ1VqvA1g0dgYSTmpR0MMFAREv8bY%2F0AegztzHx53se4fGZPOEnAL5Emhiqq37gkt5Y0DnahGQHKvEkCeJk%2ByyJTnTn6zehKCZBvBgT0FbvtcdvuPfn8bWOj6YHqkhN4ecv6NqwtkkBSeg&X-Amz-Signature=d72d38945b08b70aa2f5179f7a7f222f713179b39868995a9e820512caa6e842&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

