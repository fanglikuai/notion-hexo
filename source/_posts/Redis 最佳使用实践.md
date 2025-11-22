---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7XCIQMH%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQDvwwXWf8jAmylaRwqLq302l06fpcsGJXpS3kg9%2FyQ9IgIhAJRQ8qbBrtB%2BBSNRzYYHCpcjE5dHPjyQb5vymhVQFN50Kv8DCCEQABoMNjM3NDIzMTgzODA1Igx%2FAeiJWQ8sZsZ9yMsq3AOA8Uk7XoPp6izdnCWFU5TMd43Yn%2BkPRjbq5fg8RzIibBSgA6%2FHKcGWOQRdLXgPouWt9V6lvACYOErc%2FwWNRDBOCr1p6mIvOYPnORB2EE%2F9RR9N892OZ7fXBJCOzEkJTLvChFYdPK0m595PxPvJxa%2F7xQhcm5Z57PCkvCVGdtxGOOwk4PoZpderG1wLJ7qVYuEj74UjcQ0tS%2BrM8iM61ADLW0Fe0T4Mf7ApLixxtOHBpBxWA%2Brux1N7pIbMVWzM8m1DeazRniCaWP0QfIR1m1MDzSofoishksGdR2LerJ0dbOGjA1Y9NmLhHcQLMpTLw3%2B0X0a9EafSaukgpkydLycDdpIs0%2FP8B8Z8IRUah5moDHpYerqSydNDWdp0bO4gmfrx3acdLDdyZNtZOxyJ7vzQGyz56VsOnpl%2Bbp0gA8yhLjs630vPuvF1sPXnZbS0D0vyG2rx67sLTaHh95Jy789%2F2Tb6JowkXuPW3HxnegfETP1IjJur1o%2BTSrDGuk5UmqsJcoCxvj3gA9IJj986AJ9eyZc%2B%2FkmmQ%2BIMqVqXEFkkvmRxkifiJiq1VqXUoPJlHc8MakzIWWy2gpb2tria0W8KRp3mpCczM0ooEGgdOuACtzWYX6I77Zo8VG2PazDh3YXJBjqkAYSBduUS1gk%2Fvc07LbAJGmsZPVRlKbRwNxs0WoudkMC%2B2n8Yn1vvOYWbgEyrEGp9nFs8amgzJU%2BqbTKMlZHJIE9A5U12sldsJyvkwn%2FoCZHN%2F2Zp1syVi8LdYLevIuOMhjX9YQ%2BCs4DuE4bgahkm4QWQbpx77ips3h5LNY%2FHSKMqG1SRUILEIQLwHvgm1uX1waSLH4uzxEPVCOad6vpfDzdJgjAV&X-Amz-Signature=7bee7b8cfec90f4469e4eb8aa76a94982eac9602ac8f308748e16c8d8807de21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

