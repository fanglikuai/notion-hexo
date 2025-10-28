---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NQ7MCK7%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T110036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIQCUUBJozGnm1fd2bdikHWDEWGT5Q%2B59eRNjLmtvZgJcMwIgXimAnVdRHrx2HwE2s%2BOGMmjlnyMrCOvm8L3W1ZglA9MqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIOVysaWBZ0DUFQ8kSrcA1upcuR3vTKfe444KfVbh4z8ZZc15wHuSHpI7pwm60dtsmL9VRLbSNpRFq3hqj9wTKRDtFPCQPh%2F%2FHKH4Aw5N%2FoYvbFuGknCRcVXJ5g898Q3rbdlQu9LMU5EG8GDXKcduh3%2FQ%2FyfuBmFJYOvWetV9tHrIaCU21PEU8Mc3StBEgWuv6uKnWziUHEr33MpM6ovkpvfD2zb80ypzsYzL1JuJcWMRVchprO6aum5hjkuWu8gsP7nPkSW7v26fZoJKCgr4Rx4b5I4mtOM8R1A84wouRNLZJSGvlPr2Ss42mcAjrxr6TiBZP7FhiUtIFxEqk%2FYp0YlI6wCYEY6z2HNKB6jr5LBAjiZjo60VR3xMTfj8V7zjyl40i64dqGIVhuslvX9u2yTjnGaboj3TOfmO%2BdmIJUZmGaG4lck2LAO%2BrFQeFbWgNETZyrPLQvLuDa4LDGDRd1O%2FjscYBQyN0j8xqtVRZU9YRmMy9GbXGAllrV7uwumgCABbpdhHiehEKnhOW9uhY%2B%2F0%2BVwknFbW2q9ZA47JLAvxe7GIurkECtJTlmKCmVKoaCli930wJ0c0H8K4JWDM0xFpmG1Aq2BTicloENyi6X0xIlI1mt%2B5Da5d0725MuWUdOamvOv7unXkvrWMPOmgsgGOqUBoq%2FSrto9hWp6VXRPbm7QWLO1GxeMaae8eMWGCANBRMmahgg%2Fane5kWdD%2BNYgt4kqeunery6jrhFSs%2B295Zigd6a3b3WtycnLKXG3IP%2FpCqYyUIRf41%2Fen10Yx8wHyZMbO25fz4KXVx%2FvLW8M0pGukzCCbs0Pca4cxLWGu8WT5N98G%2BLcgho0EvxOyVVkFjA7O3kESFp1A9f41MAlnRAVqGmXXhid&X-Amz-Signature=773ab88756628a81901dadd3eed8d4f589644caa657d77683b3614945025df49&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

