---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VKGVHRMF%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpKCgVMO%2BLHLKk%2FCJDkysw%2B5CWcrLhLYqSkuDdFsB2FQIhANjo3QfxONvowA6GrbcAVGeIrFd3vOmgKGyyC%2BHhAqzuKv8DCHUQABoMNjM3NDIzMTgzODA1IgzkWjdGuY%2FJOZCocIAq3AN3ZFk%2BnW%2FgrGhhnqaccnvfSlr0HEEQVanY48SL2iujQDgcapYo0baeQc%2BVI2fksKr7ZviSF6jWpcKYFvrH7vtpP7XssIBTXf9yKDlYSxnG%2B1GbtE2SAj2rjouwvnO7A0cHfO0S0ue3qeQ11ZbrRDBN7K5vZEWPYQ8rSZNKISoW6jSJ3fUX3wij5T%2BWfPVYIq0hEWleY8aZBmBbgzdOdf56DvTWTTfzKT7dJnd1XtfGSI2AquMRX1T%2FaIkYOdzP4a2SDX73nN992LFq%2Fm6KzTMhFY5eA5R8LguaPVjgXD7LWaCskbTqxmf4Jd%2BPiSsDW7dqF8wZN7llhlazIyio8K5iMV9Tw8l%2BDHx1dqQovkPyQ0m7gkHmfE0rdceOGzYzuWWDFQfX2MZ2F2lqFUHEokhFuXnWy1we7OEWaD4mn8U8tIwGQ8HkjfnJikYH84eedFEN1bktLx9Cm98bEaEgvUFboRp7TEnjfFe9K78DlLs%2BPJ6QBXHNrc6jv26%2FtaJhnvV5UPP8Z5gBCO4wt%2FsMaqNRfYOCY6quOMYNK7mT%2BIJH4nrYUPuK11ayl3w3pW420CK1Dp78RVT6yupja0mYrWv8bEbgBTi73wfzA3CTJ8Cq2UFU3n2JubLAvgpG6zCe0afIBjqkARaZAvUK3JZN4Ldfa%2BsdwFVUVRlgFTswn0rfUGs0DvLWl5IpRL7I2I9FGfDtol8l6dQaKF3UZk4JIYM%2F7v1ckKGtolLo9Sp2QsvkiKXuzoleye8iHqmpLMWNUxRIqeO1iFgvOInk68Uc731%2BSdtKTEE32DFS4KOkspZOmxXsfFeQIgYEyN%2F9Xe2z1RSVP1Gn1s8gUGp1nL%2FdBV8jNMy1AY4Hsg2R&X-Amz-Signature=a6fd4f6a8c917c9385990f5e5fd6fcc1d6373a40d1321719f563521519e65ae9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

