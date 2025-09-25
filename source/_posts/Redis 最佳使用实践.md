---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGAFW2WY%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCk8K9cGydvFFRMNyhxLmFwptgNLx1bCYbYEguPlzpJUwIgATMDYKOnagcYmTTIIMcU2zMzrwOGizQ30zrlJ4Hnz98q%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDN0DNlghVrF10AxQSircAw%2BLW3I5Je65jLkJC6%2FIJUugdjU4OeyYt2cMpLW2Oag7%2F1m1XPAfptWW8Zqi985KymgDrexOzRBYwFmBcJZeq%2BsWWabp4g%2F%2Fd7Xo29ZxW5OGxxQ2qPZ5FShDCuA8aM5pWRn5fVK13hQzDefBTZyLG8W0lVY793m2LtexWKs2f%2FSk4SIcQzsxwhLhScy4kfb5Y4iiBnhcEnBBLsqUaffaWTGHdQBbxVwtDR4X%2B89T7mRtzVU8egUaRDEfWIcyhi%2BnIzxmLawDD0sKS3s8mJa3yBlf4DpnCWj%2F7sTPkaXo7ehkFzrLMO%2FcoR8YODdol2bMHf4gPllnr%2BA6yYMpjVt8NiuvAtex8Kjaw5cKCjo2QCiV3Kw5eojAKXqoHOFZuaT7CAlC6slXEb065FO6ugsyQbB0LkH9%2BBIWgPukYiQktxVfnLdAVb90wBGRGgZPzPz6y4RDV%2F4PQlfGBxascZEgBbdtFnwE222uxxIWlfndBTiixjL8Gww3SZI88MO0JW0jfzv4WSnt04kcUU2kOWw49rGKDtsIM1YeyUOC1rfFuMS08QQGyAmshE%2BGKGDMde0a2U4eEbBKOR8LRKuMjwZNmhnKN89ZiRo13OSBP7QQ1tAiZQKroHgdSRvdUegwMPe71cYGOqUBRm7Y5QiFGvhrd1oJA13Kb1yDNmIHt1QiN0Nu9YYiTMZBzovbUi5Nn9e%2F2Mf5qLSIFj%2FKAxlBh21cWVFrlrmS5kJF86Vp2AT22IJX90Bm%2B47FQ196je01RPV281t2ejsYSigTiC9fPl4aTSvuwGeQ5uk8687gMZ5Z7zi9fyzor7SGKgL4wZ3uukoiBx%2F7o%2BnuGwdLPVgzFezd0a1nP1XOLEiN2vPw&X-Amz-Signature=bf7b15ee624da56b85b399470a0b9bb2dd9291521cb04f33d8332545e519bc2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

