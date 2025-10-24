---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643UYHHO2%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID6zB79FmM7bUoYLQ1UGVbK0kr7mPhMjnJXGaiVlwqV2AiBn1FV%2BmVDKEN0HCbOOUcNGrspC79qy4%2F8SAi1lsPCJoSr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMc2kYs2EBz3xC%2FmPxKtwDVsX8tRw9QoAt7KaOhQNw0Z87SFqmGODRIQ8L4DyvkwPwkQNzlQclx8Kq5NWhrZua12d0KycwXGK6hFfNrPnGd4gqRKFHoGJZUIU6rkFp4PXnsEOBqx4LqmAWwlOULDxFqCzcyrg%2FVy9rr5dz5KAUlq5z%2BXgXNQQDL590qOIjyVGNbAA7AE4ZGXK8crEXQzhOXBquodac5PHaSmnchOOXHFyBPXaRlKy2ZUevc5ReDPKuW8Q%2F5Obq7bY7UESygFTZ4VTKSTrMH9yl0gGD%2BWnN2wL4F2Z0uydn9kRATCdOXOx9FfFXu6ukcA6LN96I2cs3rUtxgUvXFmAvQ%2F2aHKSLUo1n7n5PJjJ04Hf8zhJHxyRYnnRVPMep3tzFvy0KlfDRJSRnSb%2F1cwUgLu7DCu5jZ7B9m%2B%2Bsp0KTWk5mIt9uZaTeDomwX2k5RkiFYA3EuOL52oawTjU4l0938WAyTE81oSVRzh%2BN6%2Bzer%2BZ6tPq5XMfKxewM68A7qaNjcD%2FI%2FYs3yXHT2MwsGG%2B9I2O5agTfcAuVW5H8GX44fuoCn5HEs5apd9lBapKrGoEoMIIqRB6qKsUi3MwJwqbu9fGx%2FqeBIasz5MTmkZPihvyY0GcYRWdIrB9U2GWcKLGliugwxIruxwY6pgGK1f5F1fDIxtSxFE25fzGfgJQakoEAhGVV4GulpbRjP0YPuqhzlV7SNpjFLwjmbxwjjbwauZ9kGqu77LiyjP9yq5bXt6ndEmvh38NVMINirHpijcWCb5jc%2FRYua8NBefh06krARue4RB6QPIyroMb80VpI%2Fs%2Bq4iAPg0VvTA7TRSoPGvA8j2%2B3XqDytVtVIFlpKvaIfnXQtbDGZLDO%2BW0wguFxYPUc&X-Amz-Signature=d8e3570fbb9ad3ffa0dcd23a2946fb73d98cf17af60fd4cda33ca6534962a177&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

