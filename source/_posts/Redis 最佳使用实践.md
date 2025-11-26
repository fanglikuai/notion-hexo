---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46663DDSRSW%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T060054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICb9OQZllM3vj5dLXQt5IH5vXumy2q5FdN6Pkc8N130YAiEAizGyrMPXgMJ7qGWalBfqAueq%2B31XPew0K7HKYJbhwHoq%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDM5aPMjexthEs0FXVSrcAzpFZEsgpy%2BXfOk8lxv42TVCWL%2B%2FpIdFo7OBw8I7QrYBersU%2FYYSnx83AiBXMXidYVOHAUbXadW9GXtxJFmRMcmHn07c09YcgV80M3lb2cl83tYZwjfSLwT5NvXjcatfe2OdckcjvyGtFAPSOJoE1nANP1FL6AjauSJr5NIKhESY1LRHRyUEFF1vG67N9BpaYdA%2FZYsV5hGvIbLWlwBVmi8w0cNyp6gWevwX%2Ba8NrOp%2B%2BcbYhyMrt4ML1G8XGbBpqOMOI92O8h1HmY1Qk%2Fre5OttbGPXoLdoBGFO60QaDQIYBWlWP2uzS7hmjQArL%2Fafq7c%2BkfSDMsb97AIGqJgAS4YS3mJh2XpcMlC7vAivw25vUCCNTOoSerLKzBptaFYf6Rq2PKGRF0dUsJHu0jbLt8Aq0xYGcix8TnkQv2ZhmYpACPWOkWu9dMy82Py6jVThpBFXzH7XzJts581COiUzQohyRXDia5eKg0ZiVg1rPSvjmPa98hifFLoUPVOjGdFYaMrPcy4aMLLFOs7dB5NA0yJ07ef8TVdBpV6iuqz7qxlGQHl%2F3JFYwdwsNZ7zfJg3dLmt%2BokV%2BAX7WQ09uNiesscLEQbim0kpqTr4CZvDeIuFCcadVYL9iG0B%2F5HoMLCWmskGOqUBPu2MfhPVfPgR9QHA2SqWxdbO5nR2zonwXTEgHVb1MnnW9WQ%2B9Q4EHyCCtTpUQk1peKAyvfCVVImWJ6FsnlXhnCDI4v2nrohjHmIQo8Gtzmz51ANsCf%2FGNjjOViz4tjRcZ2rEM16L2ldjIQz%2BWUcWpACsQOuB9B3Nunkg%2Fqw2e7Z4uNubcbExYL391lYUoxPaswhc3qcUVxbmGrlbRmSpg1MEeqsQ&X-Amz-Signature=d1c6419f90b8fd258a2dba210b2b54ac42368f32581ec04a4c5a65fab258e673&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

