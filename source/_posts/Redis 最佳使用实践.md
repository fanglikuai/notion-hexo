---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MCRYVDC%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBvVdKdPslJOdBTWPL6InqlcMQcuXQsD6t3f8s589ePXAiBFbbjO73FV8o12hzIAp55nOWyXnuWfciZgFOWUV%2B795yqIBAiY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMVkCHh6Mhs7PlLW6AKtwDh7h74ATpYXHq%2Bbs2%2BhqWvJAccz%2FhDCkMtxWnPEXfSzmCaCrtuiSf%2FDRgCED86Uq2XJba6NgUFF1XKHPT5GZpBo2l7XFxLXrWnskTCCPt72gF2DQyyCyhJ9XlDhZ3tCNB3JF0fjftQarVIMFK1qHlmblfzsLrI1B5g%2F6kgjzp6qR7SOk307rEguYg16TjxTNACjfZpIp5z5UkXRB3TIQuU0PU8UQBFbbk%2Fph%2FSHcPEhHBZiRmdzbrmaAMKxee1600feqat5xTOb7LK2jAj2aVqcX1AEyjCddwcMYl9RbBebvd0JczaizIPVQVVAV%2FGU0HmIYbU5q6%2BLZ6ybxYw1J3inscYp2du3avsebwIywwbLQN3mxVkxMpRlWUEN1vC4vgVuqmoLwEhj5NeHnXfKBrLCZ0h%2BMg6851FZjTvXTTOHtd%2F4THfEoJrRSyqttqF9RFlnx5TBzIRpGIaiEVFGX2kqDQtBS%2B9kyjeIkO7qPizLIPN%2BpbAsyGN5tvT9bm1ffZkg81Ct8CyOzdit37iw0tnmMJr70E7BpgWH2EeN99Q7IaqquVBGtlPXNWeydvxBMLOBrU3MMBkFjsoyKX5ZDr5JES7FRlurMMv1AOO790nPl52%2FRmWYoS8YWkm4Iwo%2FvFxwY6pgG9ZRYI23q2ks1Edv31kU5FFyWJXoBQASAngp4sW61vN0rhlvaYfvPoUBhg7ydFV0jPi0VyBiEJUopaJtQCVg2NecvEMKLCLp81x%2BBK3rxraFmxh3fvEZFHSgASNnQxI3jO1xsiB0PFNgvAYPWA3Pf3MAbk%2FgSrLhtbo%2FL4UglWHJs9LWaQCIo6GIQJ%2FTJ%2BlMrrGD9ItwMvl6UxYpk%2FBgrM4hnq8%2BEG&X-Amz-Signature=8b5d27910e6fbd43b72a6b17cb2c7437642a62ab4358fdd811f03a4b32d204ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

