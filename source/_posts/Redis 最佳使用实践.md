---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EFDRXO3%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDtMFU1uq3mQRfXLWtVXM6zrGyq%2F4zpiFGv6SHxPKzUTwIhAPZsNHY0YDxmQ4tocSjbeDoVPXWMCIO5Wtpa7HSKiR00KogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyYPiJjLCUqbGbgukwq3ANqwoA3Jgt1mHPQsNVicAiPUrhNWsgnARejOPTmVCphVbHiBDx3pAx%2FpN4aXrzU7yTQBiDa0n5vtVl8PhYYgiz%2B4ykN%2Bbq3iPeWOWBpjQ0jqdT6GhL0%2Fv%2BMigpuCzsxktgYTMqypBeM8YU%2Fxru8YHw0o6cPjLUWtuFTOsI5Srbp9PFg3RXmxDWHsOYz4yZrFbtqBC20ammaUdWleiX3x%2FfrY0vP%2BZxR8h%2FiNd1%2F2nAyiCpGxvXMVNFoSVVhXJOc86nAT%2FUs%2FGOy75ENtQ%2F4fG%2FcgHEcbEKotrus%2FigudpK4p6Tr%2FAarZxBemUnzryfUNMYuFdcyZmC%2BTXocP9GCpxe7BscwB8E4Akbsv2we2%2BS8PU6NlO%2B8UkXoEfxR%2Bn0sQTltM%2FnwWjgZm9wjOCPQP6VxlIgp5g8eMnRJNNRuBOa1yF%2FA4N%2Fld8FJvhhzaLdFUpRYQvArxENv8zGMTjY6XtfJN2MNKMwGOH8EpQwtNXDaMqmy%2FaeBGq5oFO8e5x1Z2PGnnwZNOxO%2BbjG3J4g%2BR6pgxwxxFcQPHev7X%2BlXuV1PMiixh%2B%2BYNAvnQHYYcnhw0aPpx6Bx2XLTyhY4AESxVwJUt7Jpjfzt8TyhzedArbuBccX17pzE0oMdm5T7cjCgua7GBjqkARg6iXyDskH49vFYDHa%2B9Qc9N98NZlqjqwgp%2Fr%2BinQsWQt3QyECPNJV3MsQHFFwakzk4izrd7djBE8spe%2BiX%2FDgCn8fVLQUONWh%2BwRLM8okuFKPDNzhzrzkarFpVGSnJw1l%2BfdYZa2O0ehYSwB%2F9aOs%2BLwzdwUbrdQDT5Ee%2F89xstwrSAt79F6aty2cHAWgQ1ExI%2F3pPtP2zzzK0KNZc4C3SwaLf&X-Amz-Signature=7348ac8bfe906eb9d1e89d3b6ff4005babe684e42cab5e08c470a20df0e5f985&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

