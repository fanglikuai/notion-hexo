---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTLAAY3L%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC0CKMGOy2oCdC165xfiXfkBwkrQHoVBPbSpY%2B%2B%2BO%2FmhAIgSHOVhDZ9BcHcpbB6CidwKcVbyohCcCsAwbeQ%2BOHc1%2F0q%2FwMITRAAGgw2Mzc0MjMxODM4MDUiDGEgaYZl%2FVK%2BDD1YySrcAwzj7CGFhO5WK2pmUmi1Jla646MFegnGZnSA%2Fywoh%2F6mvbWpuJwh2QGM6cMk107YL3GCSKVjDTTtBcD2GNvbQ6NtPb4Hh%2F9SKA1t9aTlN9W7ileGuaNLBRS%2FwgdVi%2FqjIwTglpclWUJIieXoTmyuUyO1nmig2RtD6a7Og2uHnydG3Ddw4Q2iDI3xhNba4qyWD8QWQlkjNsjOvM%2BhKqUZI%2Fml3kpPYLJVjqdZcynJGkb69i6fCxjSsIPnSBNrIqt6YZFacWM0ScE2QNeYoxKzYX%2F47ghitVcJ8FD%2Bq7Gb43mKSsCuy4zHrU49IYxffLpYPXeH0xxP1Yn5JaR3k%2Fabdw1SPd1yIaWoaE0sZFLUbJrpjwI5PdapbTo3ExpcbR9iPlQghiOy7rt1RoWvQhN2doaU22pWKmgJrMEEKlvl29lPJ%2FF6W%2BKEc49%2BONuzP%2BOPKFixp8qkpOCyJ20Lq4fE5up3a3qz0iowNapjoySExrBdDZytSeEYd6VT9PPWF9yA7SS5uPGuHm5fekH%2BR3TD2nRIbKuZZF6HcZgzU4bklVweaw8sKAUmf2uS0Gz6mALl08Jlx9Ijdf8IqF9xXGtN1yj7q%2B%2BEbkfp%2BFLXkMvON42Fzl070DEUa9fZROZMMJnagMcGOqUBAgU4lqX5ZUDvPTponGdnX2LHXThaYt%2BVMJN2ePos1LvCyfmwqd4eUW69y6oMQEfrBUnL3pnbIyKIiW4rFlS4Szi2HSuItlJtmnbs4FgCSehlH%2F3%2FSM6xkhi6D5iRXg%2F3uPHD1OmoNL1hWMPc0IxK5VlIBEha0OvOAfEUEzsni5Krwk50zj%2BZPpEseQfsXLuJIdhy5nECSbT62dQDq87QTRBf7K9o&X-Amz-Signature=aad4ce340fcb49723e51fe5281decc1cb8d877afb66133a46946e780109ae910&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

