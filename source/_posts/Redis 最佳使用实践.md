---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YHNERIE%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIAGhXTWR2z2yfUc1XUocfxFpjnJMTmEjCPr0w4R50XpNAiEAtbSbElgpuvi1BC2Lu9dkXRTZ9%2FWRjsVjnW%2BfN%2BwAMjwqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCPmGMVlAmB9Ex5k7ircA6XWoj8pHj2rcikBBlZ%2Bw4f8xFtXlRj27guNHEg873BUj0UoxQhZSLF%2FM59B68dB9SKqMVDgpNpR3s5RQtj0X0%2FuGCQH%2B6WJzX%2FyNZwo2sMjn%2BGaBTa%2FABRqjwhTJBxQr3UAioGc4HvnZ1TTxLrGYxYS7YDPinzHirp4sgKlnFzMv97WNcCY63CfA6QZRERSkxyQIlrrUhbjHRHXhQ%2FZlfGs72qVrZORAPUSfnOLjDOyqrtGfiQyIol9lAs1WOVugQxV1P9zCctMq8Lqs34XToUbhMGpB5UrPMaz3zl4IyCxqEG64MkM0fgEQdy3KDx9v5zu34Pz8OgAYdfr6Q3%2B3Lz5EBHGiiWVnGwHtAKxS1QJUfUGdX4ieWxFNhNRhULVrAyzpkQ5VYG5SIoWAJDSdCiriNXHBWgJsvRnQNXLf9eZWhBcGoLCi8WuXdFOiQjIPhSWJQiEjKtBWfbcp65hRPV4caTKAlBHfynM5E77ARmUXeJGxkgvF8sZL6vmwzeOv9PATsG24NDFaFHF43x397J814FPybvZx%2Bb9%2FYAVrO39b5GWcfqP3dWhEG9GXD79zKoUdTEjnyLnpYd47GmmpMqk1uYjqMgixXJk3eARwAP9ofLkTPCycVR4%2BLWSMK2nv8gGOqUB%2Fz0k5rx2OxmjKVO9SKFa0s1fcqI4HalflS6VRNewOViuCoOpfUxo2%2FPTxWGbWWiBBbW3%2F0nFCDJoxXHUp6l%2BpEfpSU19SNnCzesyZ3GctjSZ8qLcaZfaZ%2FY%2BBe31jB1jcIeHfUHXcyk6UgvKHFHmD2rmbxAnK8r%2F2rgy6OZ0Z5nmFE15kS2YndwhOXCkxHwBxkj%2FdGJ5xUuVC4MrwDuenyPdM%2BCb&X-Amz-Signature=fe7c0d3e88170b82ecbae887c3eb079b01654f719dddb71d9636551241c56d4a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

