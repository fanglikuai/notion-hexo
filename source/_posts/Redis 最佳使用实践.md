---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRJUL3A7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIDx0q7OyBUXLk49VtYPix5m4JvFlXeUGzIpnhoObIlkyAiEA2i%2B4TWPqi%2FD4KZuLY3zzJSEHikZ7RVcRx4wO%2FdawbWsq%2FwMIBhAAGgw2Mzc0MjMxODM4MDUiDLCLJ0Evh%2Bzu%2FCLLhSrcA%2FQ%2FBvlDxsfBQlD92hjgthKXF%2F%2Bun3W%2FltiAclFrFqxPL%2BjFGskyjUiuJmJWEttERjz860xMc0aDJzNA8Cmt52sH%2BwYzOzfXUMebyv5zC3HBs3EBoIUJmXINbGDJsN%2BlVOrSrg9ExM7DaHhOk7Rf1pCMYhYW68ALh4ds0sdsyqoIAJnsfLeo%2FULtlo5r1qxTyn%2FNXi5Na8YUeqkefFt6wolRBEnWOMFt9q1ZWst9mGSWpJf%2FtJyQlL7YT9fb6c8QAhptzWaeVZXE3Bytr0DmDsl7d10LXqVDTHLVSXL4A5JigZnyQPZU24QfHvLkm9O7n0kl5giGfFhpwMSfSDCTN3rv5Em%2BLmdcw3tog18T6etgXa8rJuGmWphSDj2ZZo0SX9HxfmXA7DZYrhFNRZ0ptrvYIny1BeG8PHOLjyTetb6x1voakqpmutQhim9%2FB%2FeP45sqcYHAxiAmv5PXl8G%2FcR5mmb5ih0F0gCjfLizoPwZG5jxht40vybl9Yiau3NuuMqROlgD%2BhOBAXMSVATLViHLfHlk89hiLCb8lbMXBYzN4VjI%2Bk%2FfpycRhRXqzZkvQT%2FBCCOw7GyEvvqTRZmOv8G6NaZC1MSvYvmarbqqhW84qk3dIpC0uE%2FOG7W9zMKTU%2F8gGOqUBQ25nirfj2L%2F6R7WcYaEzKsyFYK67193P%2F%2FWCax2vAPMF1Xq%2B624vZWZDb5V3Sq0ZRHeKYbiqMQp0k1gDS6W4W55w9T2UQ9rzg6kMHxyLO2d9jAVXWfWZ6Xx6nyUKDltVPlbIienzK9o44EwU%2BYfF7mXwUZ%2Fb5ztfsfrtN1vlgSqesHhqqjA7VQQPKaPz9M%2F3gIpAdbrGds0QlNuJTRfSsP0SPigm&X-Amz-Signature=bd0c59ff41b91005bdcbc1544a194f23f714be911208372e957851249d936813&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

