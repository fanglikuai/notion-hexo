---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYRQEJEP%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T090106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQDtMoZYJh6%2B8Xe20FQm3%2FitAmpPJYguYbAT7ygjJnVsXQIhAPx0FESnemgeSBOf21kE6Kj4oV%2BO8JnccA%2FfTZ2X7Q1vKv8DCCoQABoMNjM3NDIzMTgzODA1IgyLrmfv7O7UXy%2BqsTAq3AO%2FQngz6N%2FUe7fAu6AQdvvz3NIfjmztpQQdvyU5VMspBEZ5c0hX6KpHIHBApMcscDq%2Fny8Pcxen3ZFz%2BcLSXo6%2FuNbdiUOz%2F4dqcEIL9qDhhi3VTBELz63sYK7xC%2B1XoATz5SL7aXY27U3SndT075A%2BZiiDli3%2F%2B68OMA2ZDnpeyho7oivfpPmDpfoDnRKIctYqg4TYcb0VfkTBcCk46n7luwj6o55giZmYDrFBjlx99Nv8Vh9wkURVVT0SJpzqA8UQ4z2Lk%2F9QRPXUibCXfnnXg2Fpy94hsQ%2BhFc5xBgWuw%2BQiARRIwfUDU%2FXGoRnqcJIi%2FxxshS5LvxjYjZ%2BriKqittmHRnrM77lsKAwksfo4cX7BIvekzz%2FhwW%2FSKGwPeag40lFyzq4GFiaEBqJUk9%2FU%2F1dcZaonuwGWSlT792E9%2FM8VPmq3pk3OevUU2Yy7ZXxiUuWldU2AsdUf4RLnEqYs1NYwePuRMFzVnL%2B4IID%2F632S3kXqRefR8XfIX6XmToOFG5m%2FlNc%2FxBdfGioORkb6Sz2RBehQhfvMxijAvBkFHKqShGfsJGpHqs6UeUnhBYPSDod4%2BBWjEEkXasT9d3bcNuAQ3xtkskWzvVqVurFCqK%2F7gGL92PtKUMQgZzCfuOLHBjqkAVLzIPHLVlItXuhRDhy47EABnfn2wCQRjWTe9VLLIoonFjCU30jKm%2BSyj8%2B0E3HcUaVsXMdUPRCvqHDox7kYEgA1IdQ6zaCiogHtiDOJw1uFC78dywE6GKU50iMGVUyNQLOMJbtlVTBw60X4gBSuuOIWfLdsnkmjJ3qCAzRauQQHm7ae9thLKvnd1wpv1gapzeR6QsMn27oL3AIMZXL9acH2TPg0&X-Amz-Signature=f62dcbbbd6e80395a28aeb0e2c10c435a73ade3da663014aea909e554a1e5cfa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

