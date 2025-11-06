---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EUMGBNQ%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbBauEZfW2l%2F%2BT4b5tpzejmfv9iE07bEH%2BtSK8sVh0fAiEA0BPyAjqo%2FS6FTVGFC01yw2eGtjt3QI8cyd%2BxkfrbLPAqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCi90KLW3cTFtkeZZyrcA99h0u%2FmykVTzZtFR4XQLlG30N5VP8mpGNy92oi0rmiJnsTciCdTd4YQw50Wl5vTwoTPqawmF71gBK7lvQ%2FA91Vkd%2Bx9bWoT4OT1qnRBHV2p4fk7HU5WRXTNVvSa8dClYYpn3ud6TDAHc3L2jZyOcLle5TgPwzDzsCWzj79DMEFkU%2BjEXdn9oNiPgcUsGmKyVrgGJaDr15c3iGBpqww0iCVsZnOkP0Ludo5yf%2FjM8no%2BUe7VsaC%2FezwP%2FJ0G9zvIzM%2FpC%2FGNXQzae4Fuak4mKINDCqAknNpMGoravUA5DnDTRaVvh%2FE%2F09RMf1d01h0HUSRyqmsdwRC55p7AOrncbu6uPTXbrewsNsXT2eQCmD2y29IsYDQONUPvyDj2IVoZ2a0rSyC%2F8gOr%2BLqRu5b44rk4JJVXZUwNI5kUkntvTfA6bk4FcU8AhVrVDTXgwVp%2FJe00FsHgyv3I0uQDnGdDu1Tx3H3LCihytwzPmW3xxVaiZh%2FqFBf0n45Nclu8H38ewez703a1zBPqRG13zOFVRlWRdf71hNfWr9yZhFsxWAe1uQyJiNLzuB0ycQE1NgB30baGS0EbrNItLbqydk7mxezplXDOL%2FRRgFzC4OvJT9BUPNxDN42c5JwB8wbNMJugs8gGOqUBIJVZtbonBgg9uzfPvk78RxUxSa0EhtHRJ5NLe0Z6avNhOkbZRN8VQhN2P1qsgUmdyj%2B92qtLI3qtEhogrXFDRksELfCza9qFWWMwUFbm%2BbanDhd4jPJxRtI7Y5Qb%2FDQ51GnfB0awks0X6Rts%2FZtwYBnDmBn75HAhvVOd5b4AWRfv4XWAjMVK8ObBPwYaO9fJlPf3fxXgi8Cur2oTYI8ceOCQw1Kz&X-Amz-Signature=2e4880401a422d673703358ec9197dcfb20ca00aae99a0e1e1f8a6d6262d2cbe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

