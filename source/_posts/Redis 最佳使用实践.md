---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663C3S2JXH%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T090053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAsh9bW8udsh2jRVJ4wnGv0JX9ZGrvNkT7ybzHweY9%2FXAiA9KmLYhw40NdbJkC3eMqV%2Bgxn8hFYzXnA0P9JPdmHJEyqIBAj%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM38Rx0CMerhJNxOw6KtwDn%2FLpD61c3e8uASyDb9N%2BhFoQsjcPv02UkV%2Bss5kWmuaYcLI%2BCkNu3yZuWg%2FgQ8zIN%2Ff%2BosO4BbyHQhKRdMkKhQVHAurojVhMsjF9Rxy52%2Bgn%2Fkimac5Qx%2FJeGVnG3A9Gh0jpFzz64wxvgknUSHiu%2FwX2KuzkJZlD6WPHGHc9EnyNraBhMP3yKA2L9z3wI%2Btb2%2BvtJFSsIRhfD6%2FHEejkmyYvST%2BLK4zP%2BT9CcogcevOo7g7H7rKnnHXv%2FNN7z66euuw7MZZBCP8RJMHzU%2ByGKrADsdcpEjuTllCfdmLpje1f7cp2yZEn74mmuoxJFC2MaFd93igUYkscEP6xolf0%2BoBX4bGpLttG5HcsA96Lbmg3vx3ucnwCAz4%2BzrQJgyBtSJ8rdInzFLnUFaZvjKZancIkEaZv%2FcNstG0e6shFPNO1yUPa9N%2FxDXClBPa1alQf4wxuZx1CGH0CUV9FjQS%2BMRLUUYAgCWwibM0d%2BjkAh3%2B%2BO2qhOTw6fFk4i7PBkJcy0kQHQ9z4t1mz2IhVMZvTYWe1c8mm%2FrvpfrcCGAo3yMmvIJpg3yT%2FJYH1rYZwKXgJ1dBE4uvkC1Oppfv6l65rQbzlLxkVVK4g2oRGJGwq5phOApuB%2BvH7WWfdc%2F8w1%2F69xgY6pgGM6yGfuSrzolfAzPVbgWqW4rEkLarqlVfxi4%2BafGBaFLS1INIfEnfJofez5ytK9Sbem9nL4omUxc5Q5lT%2F20VO0JD61%2B6zMOr7ph2DNefdyJA9AsVKlJNCp1YSeyX0nyxSeLkb3ueLRvRl65QQVGp9%2FOBYrAr71CBv4txMQf%2BxNMxFlFRcfSbXhzXjnobOQpJCiR8go1Wnh3T7BaQ7pOe9PN1buqGc&X-Amz-Signature=5e375b03b6ef1a5d4bfdf52140bbda689110c632810caec2e62f085c290114f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

