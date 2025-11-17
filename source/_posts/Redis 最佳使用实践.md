---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WOOPCLKX%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDn0M5trbXl5Ye5RqKyIr0921Cxbzlmfi8znaHav8oC4gIgWrXT%2F1wtDSTtjhcajn7UUkp0GAsmfl5T4ARCgP4iubwqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL6zT%2BDxVwFWsqLDbyrcA6UcA7QDstEiDC572QjeP24kA5O8MXGmpzFcnUVco7XpHzeId8S2QWKn3H8bKhczRBFumBMaWV2C6w5l5StlwS1gidX1oVFDBoA8xJiOYpINa7RvtphJFPuo9JwQQ49Sis0Buqruz1LrB%2FS6lQ2FwD0lrDpv8Wnc8QFR2%2FFagexsvg7utCBPTooYPEUObv3z0l%2BxoKzI6K4C4bb%2BDAoJa2eYL2%2BIORWXWehjHgUsa01oBL%2FAQYdPzXqppW1A7sfwoRfkyX97yqrHOPIn8Otgv0mRe1cv%2Bxhq2pGPaIV8j8hyyVDd65uNO2E1MqS7pmUC2VyXz8WaeZV6js5u82rtRzDpWkRvxdw5LrwAysBoIfKH8KXwmOoInfj%2BHlrxqx0rSn1uHy3NxStHdSBb6CeCoEItV59C2q4ZDSYonpgDRmYrrbI%2F55n0szjIz6LXbCafPk8QO%2BnlU4gPtAc4f%2FQplpUZsqF5azjR1DlchhVTnr3SGLPoV%2FvGDQomLcRYHCORSX301e1rBAPZY9OeerNqzcdSHXruaoDEue3U2%2FnRMrMgrn%2BKCGA%2FugF5gLHhYdvDz4tZTocoCU7nUEbiO364MHyay%2FZOwoWHloPMWci8aLYEicQRUiuQ%2FY9ivr1qMKGm7cgGOqUBHht7RCJdRH9hFAiUaOoLlIKm9mRSxHnw3kDd8O0Bl5dfg48%2BsGHO%2BmjJesy8vdt%2BDSY7t4QQFOusaTvLIFyWy%2FTXMbL%2BsmyJl8Ay3iawN9UZwO98d%2BPmpZcrzEXFzNkVa%2Fe%2FD0lCKT7F99e85Zr6VAjl%2B561O%2F4tzYRxeCG3o927ScdT%2BOLqscqT1pJcXWxrYj0ID0AZb3SrqsOU9TelXzeDAaMD&X-Amz-Signature=5df8b6e769f7a2831b4d970e24cc3d561456cc361bfb7ddf2356c1eaa003c75b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

