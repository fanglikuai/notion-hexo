---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ULLCZIET%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJIMEYCIQD9PxMAcfILWRCOGHXfagzqSitYSftvq4jYRsOEQuh3fwIhAOHit2RpPUCpvR69skE%2Ffm2C%2FWRM1%2FxN0WB65vW26zwsKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwwQeERlCaD9u1wDNgq3ANqQrV6punJBHuVJFnqCdRnnxzDnFjnEbQjM906jtBTMJlwrxjSznn0Fa2c3qTyDS0Ad1%2BmaMB%2Fnsco9Rrco3dWfUymwRCXDnL5UNPzyk5Mk2uzcHvHLt1wopBbL0Nw3mi7aaTx6k6nYT6NvQ2imfUfb%2FCSOyA7AbE2pK7%2BU0Iuhdc%2FLA%2B1BlIEZ3S67q5zqJRkXCNUQ%2Fv7kVDw2V%2BnSFhTDl9kEkX50Y9dg8zEuMDtz6cAhrvQ0ZrqE6Lk4S1JN4eCVB8eXLtgtea%2FlHzb6gSOOIOWZsFoT9PFuGjbqO6AtyaEqxQZ7hrE73A2oewHYROzLWk4R03ydtuEUa1sHbJOAom2eO3Rfe%2FGo6gJevmpvOcw0EZBEorFahpPioCh%2FBYaM1zwx8qOIyeIdAjQBqUx0geq7TRb%2BT06ZiBUOrQQfqUyqDkNTn66rKaRUSEhTo%2FcGo%2BQyqAOvHcTUgs%2FOssHqqGLTqKRzkoSMfEUKSkB9vSIiVbjs49UlF%2Bcyn%2FC3FoSN1RWKQPVDCcZFxuNDTFxwrJSeLeXkFNJrNH2DgMPbNKBgBnuIq%2FKjX%2B86OBa87E4OYbjmHVHq0isFFzSnBTBzAcAyEA9gjC9Rsv4DqG%2F4%2FmB5buPAehMljvsdjCt0LzGBjqkAZTo45BCzYRZQv%2FJy3tj7DGxQLjD0rowUMy5QVsMvdYqxN8rr5%2FpOWy1qME7e5Qp04xoTAogJJ3a1sQsbDW2IwYEtNRrl9jfjMy6RoIGn9QuUNr1vh2GfoFRu8g5rJax813ZWTZYQBLBMorNvssWzCcRi9SKoRtm%2F%2FCd6sD9RBiV6IxvvVsrfUCSzyh6SADtRY1R340aJClpgpcfOzbA3OuubKUC&X-Amz-Signature=7ba4fa0668c51c0416832f3779c9cf58e2b98e121b7347f5a9c815382f19d503&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

