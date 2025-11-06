---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI4W3J7B%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T060039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClGm6Md5ydTwUVENuzCPmtsG6jmY6ZQtyPanX2A3SVngIhANrXuQBLS0EZh5Polh9%2FfURw4vi1wpYQFTMGWpPT7MFPKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyTtY3e0LE1ydwvpKsq3ANv%2FlUFYRgk5RHcuP%2F0aUzXtijUo6elvgEnNugllBaTW%2FgF20bPrPA30xON3fR76iLYoXDeG6LMwJGSVLrDlBmuIkmmPHOeKWmjjja5HCcyPTrTfGCWkdpSSywsyrLxztWDgzpo%2BmvJZxb28SHTGp8tEP8CgkJtscjSlndyek%2FYKeAmxHT6rop%2Fues4G6NaAct13ubmrb9isKMVUPA560an5QJ8%2BH%2BWJUsptVAo9D5CbX24EoGeq2smY2jCkmhdYkTrsaaAm3kvx1A5RI%2FAS1dj26GrEWJX1J26mmEpcR2AqvjSyvlUdhubzWnveTnyGfyuOOZ8FPXu%2Bq0soDk8KXE0yYmrWCm3StNeBhGdlUgOHL%2FKIc1NLrKe4t9kFXrd1UXrqX%2FO3I6zpiXDjtj5Brfoo3%2FKtaZpAaisXtQ%2B53SyjoumxBWHqEEF%2BdiasciVHnAmjayS0mJoNnQ53O0aTDehKb65FPDnM%2FYm1jXEHJD2UOv9jPR5j2zB%2BLv9TLF4gl%2B7%2Byp95q%2FO0hDP50iwBfr9l07XJgJ7pql%2FxwEKVaX50BGIdwvNWznrkyTFC8LTFykcZDSU7m4Hk%2BmKGzvwWe5pJiOW%2FKgpFWtZUsjwSr5iLOg2ERyRd0bdArsvGzC5urDIBjqkAfuoMGN%2BH4tpVp5%2BgMoBYEmFpIuWnj1Zqf8fZzxJ5u%2Ba1w599jS9ozRrvLfquBfycBtCCks1iTMosmZ2Nhvn8mQv4PwsoIwnNwn%2BfBbUpSB26AFZKktlhbDPqNyRlb4vqlMFvLAfbkapWtxLp0%2Fd4mB8WZ4mj0HGG%2F7Ilg5D4AR9pJy0Yg6kANXPnyzr0ZvZ9P60NLdKco%2BeSOjKHZFiB4hzHoCx&X-Amz-Signature=f322fcc8d4b09686cd062018443509531548633d28a9750901e9533eb3702f74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

