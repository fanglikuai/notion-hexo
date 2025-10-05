---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3TAQGQA%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBhHWgHhZo4u7HF%2FE7H5uZYbIWGabXvsjWmQj%2FIxcyWEAiEAyBGu%2FoWkvZpJFJjJzY8%2BZxeaiFhVVcORtZsfQVwv9e8q%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDGKGkbvxoUaODBP7lircA8O4utnBRFpZqoq4vkXGMtEXzefblyVJ%2F17ZXG46eSOeVECbQGUILTFUs%2BGDVzakFSYuYpw3gDq%2B0LFOJOG9whSGPLFWgvVACV3Voe70trHDweFinpArav65BO1wnRtwxmrztJNKP7HXO1kfnN569%2FkDj2%2FE6wjaycr8IzHNCmXSHdTQF3FbgO%2F1zxtq1EZcLA7tO1gUKImYD6cGN7tD4TWMd7n%2BSZ877F6eBzZDIws7XFhLxcHl7Jz6q7XS8d0BaNJ72gJAivjw8caKLXSs%2B6UxaqyqVOA4XYE6uWZxAXSafgg2TVLT6CieSf6JHnFpHvM9tXfQvZMr0p4CEvLbdfJ55PqJH3HWR9JrDPncQczAF3dp68Xd65z2A%2BnL4S62YV43dVfPUdk5wKL9YbyNfAMCJjV8lgUG4HChd1YJ0wrx74s%2B6Ej6tFwCmczbBe0LG3%2FXFnRMe2p2zSomlvixxuolJI0vtsS3GIqwUuW%2BF0zyxhfx5uYI9s1opF8akiU9LjOaxTmX9q7v9VzA0NIuAamiOzYLYT9ChpofM%2BRN3da7VzXUoMcMPZgRb7e7WSgB2ZqPoA5j0L%2BqBpPEsP1j7DIThDAh6nfwnpgz%2Bj9q4tCn4O5yszcuxH7tFsQ%2FMNX7h8cGOqUBUppSieK2GnrToN9hckIzvgvnx61AhB%2F7GSz3VhM7aeYG%2BGubDKLXkJIEHxn64ty0VW29l7Q2zDY5DWszFgvjuEKMLgR0vyDBZE3k57blt9inrtyrqj23bowbNWBKrgU6KNsaOnN47iCy7aGaxaHMrFU3iX9Ccrq6jhP%2Fvtsh1DLRq7oNtsKIexgYfOEsAWmqnf82Vdn9Cva97WAEK3JN0%2BdeDj8c&X-Amz-Signature=3a851434f15420540f8055232a6bcb4a0b849f3db7201811468103b3c2b178e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

