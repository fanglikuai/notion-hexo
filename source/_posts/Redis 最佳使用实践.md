---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBJDZDNW%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFnW2yg19WMiMvSrBDJ9J3dBrdruNvimTpiutklEGSXHAiA3fbACvtk50vBLNQfGMuLWUi8QoUNgDok9eO3RhK98Yir%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMvXltWVjmodnbwjppKtwDt7NEbEmQ%2B3UeUOF9eM%2BBAuaMJ9H6aZDvFhJRs9ezVLZUzPv8ja4dvcz6NDISll5WibHGfaDfe3Mle8QxH80%2BbBfbtm%2F0Mra6xtJE2k7JhhhRXAHSgHUXMu1yQ7CXAO0aN9TkHE0lZ7Apxael6UmSVa0HDwQaUW5o3HqdgpI7ZKKBxB%2FfF4oVqBznn8weax%2FKT9DeW639lo7Mey6Biy%2BXEaDsVKMkFPtI3D%2FA0URc5NuMAZeyE60xUP%2BpQRQ7XdMJds1M4AZHGbJoBqJaewzvF8UCp4d2JZDEXG4jBauKYSxYiP%2F7ZnlhhcqYrZh1SVrrbVX%2FXA7iFpiM6rhgiau8dXk6pHUuQVVQK13aIE1ucj0NMplYFmYDoKNv%2FazpZPf6r%2FZK7fcuwrZ%2Bpj74WyhSGUeVGxOQZfsvYFBvHarCdjdWGBaswpxSc9OPra6pDK6VjDrJ8vlO6MTc4BrHBDAjzPqfkgOLyjrofNGWJUQCHx6uK6M5UNqXiCjMJOR3dGtNRHEo%2FoJhndOC5dTF03Nv%2Blo14geXLAb8yP1%2Fcm%2B8QvgfG%2FeZlWOcGQlN%2F7a0LU8o72cYYcIMFfp4hqzWw7sbybhNdnzdpE2Uv4%2Bbz6lmimtBH6pmFbdHT1ZqcHIw4KGeyAY6pgHUM0ggAL46QkixoUgAIakhFJsceIkTyDvs8Y8cjD7vvJzHqDqyJDupRgI5fq7XHHgxwdF1VKmhZ8vlaK5Dopbb0r3n8YFWNbUGIF1TK9kWCvmsz6RHi8IMUoxpXZS5XV3cLsqHGImI%2FJjO%2BfdFmM6WQkJJLGjCfzy3ORdKvTbWWQYLorD9uMrMpqitfKItFcDOqPmaaG6BiY7hCYB4bmTq6JgkWVzC&X-Amz-Signature=496b4cf621fa7f907abf517e9cf6064b98ebbda5c0a5636065fe55a2c3914812&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

