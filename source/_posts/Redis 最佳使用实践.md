---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664GTAJSND%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIELxnRB%2FFxjArzTwDaVx5UXh8gfKOez4x4GG3Yr9S1R7AiA8WZhyOvh8eJJSKxty7JcC%2FnB2R%2FtD%2FTPJH3B0iBH%2FaCr%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIMaQy2WwzCnvI96nFRKtwDhoNnyPrv%2BjbSsr0kN3kVR0Sd%2Buo9nwRtrLWtQlGt7f3nzQVqJonKUozrVCvfnj0BOTUtVGEb1FvQMkYt%2BvtBI4kN0IVmkUkVbI%2BqFEoQJ%2BvFZsmPe1pdzpcO4fXcpmmwE0g15byK1%2BfCMN%2FY%2BpaTYHOQ4e6YfsaYOxlRZAgUotAwqUfyKE5on7XMqGhIB65RCf7dI7GuVIcOQm%2BLYv%2BYXSk5rVMCtULJPaghKwjQRg3ZZljuygUB0Z8hnjI3BaYa%2FPcCE2GNTYMkIBCro6Esl5ZXoH4kzZ0LXl3Uv8HgJDvKVR7OP853LFXywPtXBtY3frs%2BobBxkwcWadIwJ49uCib4ug2csGfjr7iCO%2F5wj2wagnjD%2BAokun%2B77yETXzvILpJprOeMiRRGikNPzUirIlzvpuDatXKV8Ed%2FfNmfb0M56ntXzLcockWc1xmRDipOWdv5XwdRAORKj9z40SM6fYqY4PMmV6vwA0naZeMpRudOomUdqYrsYAhYcAWxf4J%2FNgYsxsO2VcDljzTLQ%2FYIg4rYy%2BzVACqOh3hciwVwnJ55cLB%2FMzR7Nf3V9FfiezCueI3Xd9rOxf504cVy7A999%2F95v3kKi16HIXDofYE35Hwh%2FDFs%2Fpdwa85Pg8Awh7iuxwY6pgFAnL%2BK%2FXg5%2BsJnGGq63VPJo7k2qT1vr19DfA2AUAMBlQKTRAEO2sdBkL%2BVgr%2B%2ByUyzLtXLzd6Q5VEgw%2F0kuTv86%2FJbO7u879ilY6Jfxkv5AbaLX619QM38LUFLQj%2B4UpcEBXndm7uibYPg2tQegRKJqaDeJizP7GK%2BZL32CmsFAwZHupy3O1EGBDVi61DEdKtCL3u73LV2SxwOwGrbqUjeHCx6UAVC&X-Amz-Signature=7ef53bb21ec4d82ea4bbcc8114886da691fb4c7fb7cc60bd49af0923e22e52c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

