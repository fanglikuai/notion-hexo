---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YEXIWOS%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIBneR9UHwqWsqdcLU16Y2nb3z5Ka%2Fq9riQNbK9q3l6nuAiEA%2BoNJS9WNOKDaF2jWQLeCxAULRjRbsZYHmTyBBy1uTmgqiAQIxv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA6prd%2B4IgtreJKXsCrcAxjo6De%2B4mf1hX%2FsllSMvr9vpBQ0x1KyQzj7EnEAd4pSrI9AOxDhKEUeJAAOCauzli2zkIAheKu2mmSUIAijWEKFcCVb80GmSiyPsRqSKbn0UCKwZR%2FaXlAqKgyccke3z1j%2B8cfuI7YSrSy3Wif63XB0ZEEUo5RTh2UF45Ki9uVqhqVT%2FcbW5OOIklDIY0v%2BYAaGRFBuCOTNHHnqiIk4LCYLCJrhwMqMlKeGUE8Gkce6ehT10zMjCVG%2Fe1aOgLglTTkp2%2FRMdeWgNdu7z2BdzYsO6zSl8zF7AfvcgZRtN9%2Be8NKzuSHaK%2FWglLsi5zFA1PPo%2FwUGfz0B%2F7tL0N4%2BLR8HH%2ByVq0L7q7PuB85qt%2FWQRBLpBEZIGVcZqaQ5YAp%2FDwqhK%2BUwrVR5mR2AI8jC%2BUjpimFjUYMnt4Tb5OvyLYCwR489W%2Bc1GCxgeecK%2FO%2BOrrx%2FkCnkRwkb6erKFO%2FpaRGWDbqhF7AdXCZ%2FYlhGbDnDEUv00fPWA9lWmgmSH6TlhwM8MUmFF2v8PBsHwmKH5bmP7eWxohv%2B95nReFlUANXIXtaVK6vsQOI5wBO0%2FMWl3ajioMEovFeUYNxtFetoQ%2FLKt2cZCy6egeodjqFJveUrgPXltIqMprKnsdwfMNCim8cGOqUBst7IYRrF9euhR5yBbQC3WjF2o3Lxv3KSgyPJg%2F3i1vOCqsJbJNNcdbwh0MfExBJVGKKWuVrgq27H%2FfpUPrlsg1leyc0PoGkdEIAM5e8edU7KvPNAZGkVheC5MqP71j2r0QfsAgFCKn0w1vPzooLCySMmohKI70MJBRgkNWZNPk68qy8sSqPkZuHcPBMrcoAzNMhdBFGx4maUwJxhSlTaFe98A9IK&X-Amz-Signature=671aa725dfb70b69e9f518f1c4b2052baf46ca6ad1e1a37579a89a0492188d07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

