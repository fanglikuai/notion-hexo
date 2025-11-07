---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z74PLTBF%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBDzKdxaC0ZuLBsKltGi61zzCyZ7s681uEjIJT0uKHSrAiEA6ijtly%2Bu8L9noCXGFU68Uf9dU0LTCXQUhfX%2B0qOyvz4qiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHMjTv8AFgyivebSACrcA3uWNjTzBMCC0OLPssS%2FiKagyBQs13crB9qW0RAK%2BH850QMnSVFetCGGOlD%2FYUbGsHw6sTROL4CHi2IH%2BIYE4R7xpfsUvAv7h9LT89tZBrfQ6aVUvL4vT40fLnvx4r69p9bNZXh8X%2FZJy5YCMpoTQ6wSNngl4SJoCF%2FuhDS6B%2B2pjDgK2nUY%2FS6wersFFdAabZ%2FQ%2FkcNIlvz%2BOfVxUlNeCLMUHMYOejJ4OdFH4FGq8DdQqszwVtGbJ75GJlM0hSwK3%2BnFd%2BICfrZWtROvOVVbtwYhS1xerrFbBrNZbHbETDgLnJHVUpvPfl8u5tctHTVEw505ZPgHT5Y%2FMTG8wLxorTkchi%2F1T6%2F2ufFQyAUw59k6xhueZFiEDBnWuVbMy8SgQrCFH6lcVjghCKka2oJ0f%2BIag7jRvuk61QSrpMh9%2FP8NyGyYvE3UkfnqC%2BrwOG6ZkQGaL%2FEb3LpvSJ3R8uTgnGemVu6Krv6yDnpoHBwbmzZC3ik8r5jwV6I%2BhmVYSdew2BdW55n4quZitNJyqlJigLgfU0o54ziG7Ixfdyqik2g%2BLeaGpCRfuQL2ZjVlSbGLxdC7HMeAQqL%2FmNo%2FpdQeJHaA5TO4hWdUzsBWUP5tc%2BqdjLMtY2laoYynFOkMJXFuMgGOqUBmb0U3l1fD4M80X7Wa9tiaQgZS%2Btl30x3YunZ0Vp8JFjx5qXufSjWxacx2FJ22%2FNMYCmZscvoq2rU1z37Ds%2Fd1ky%2BcZqHhSJp16PjwZlZuly6o8UXMxVohqLnTc3rJDeAvmJ%2FLuV4%2Bdy%2FjRiyFNECia8Jn0%2BT5amiI6kyfwIZtJ3IMz0kLIFuyVbs4WOKMlcF3UTzI7DZM56cYIgIt%2FuXQGrSbfRv&X-Amz-Signature=beef8a8da21f5b391c301d10f987bb20b50e3a9fbfcc273f273001c9c7918def&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

