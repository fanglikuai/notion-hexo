---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662RQNLPAJ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T130112Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJGMEQCIAuJbb8DJHBsoFmfJUAq78zWHMX3B1F2h1LdEDMy4GY5AiBGWQCKcJDPLnV0zd%2Bc2KnuVTq%2FSyFMpd3SqMjtuZR2sCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BxkJW7m8szvlTL%2BnKtwDBRiQf1FXz3ueW5R%2BFL4SRtlR7CB3vqrffR%2BXQHRI%2B1YjD7Kok6JtRtUbdzkdQpYjpe44ujIXmTrsQZWeS8%2B%2Bkcs4uHn8XdUdF9ZA4DD8lRGgho9NLdpQR2bTjGK2an2d%2FRThLm1r%2BHZAG85HV20KhBF%2FDaHsQsMctPh7PMPFUzTLTLQLEbRE1Fli1FrdnbWKhf5cLFCGwZreiItK7lkTNw76K6FQ6BUUjE4mh8Rqqc%2BQGriiaAHRfPHGfXC%2BoyoHdYN4NKiw05kWaVSO0D5kt0cYSt5DaKrEeQBRtA4b9XRLrkG0j4PZlQ5%2FReVSBSBeXpVCYR9UY3x2NczJ85JD0AoifgX3k6CXvL9K%2BJ%2BiUaXHBGeFerFL%2FWEpi5KD0dJFoZh6n6Vdn5UelW4Mq5tXe69TfSsNEKtDXKn7EjZI4jdSfD3YpMNApmrr4AUEFzhohEXGRP87YOp09cbaBbNg76cQAuXvXkRVshseb%2Fa7V4AhVoJJ0DKGK4gJHCNqXYbd%2B239enpfzXik00ZQTGKe6j0i3urd%2FhZzHxdmXHXjR2KH0AJoVnGo13Puj8Y1c5UQqpNLiLGyL8xUacrEP%2FhF390rN%2FUV5vvfFG1JNNOjSWX80oxiVTL1wJcu%2B7Iwl9K0xgY6pgEJQ8vx0EmEU4G0CQgXenE0RCtlzHwoxZibIcRd8dZA%2BnbXA5x47h8tQyYKXNG%2BcZauniNyCcOR7CoUcER0vcPm8pbzCGVxlO%2BIDaV%2FXZ12jN%2FfPwXXo%2BUJ8s0hK1Q03J%2FQk9fJORov%2BMTVFuqBgicX1XIiv%2BPjlYABY%2BZ6v7WKLzq1RAIZQ997S9Ouw5oWvtKkmjlbK8F9Ye7P%2BvP1TWjZo8t6dbNm&X-Amz-Signature=599952ce8ac941b1310a618228b0487a445c03797bdaec524767e5a1192764a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

