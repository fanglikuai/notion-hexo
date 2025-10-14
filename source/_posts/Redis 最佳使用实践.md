---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNBJX7FA%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD6uwOtUAWYwa5yULVpaWIT8O5m%2FjTuntcxN59wTMV0mAIgOBWhFT4basiqh%2Bs5YXl2qnIgHGuUIqeTVGpA744y%2BqQq%2FwMIXxAAGgw2Mzc0MjMxODM4MDUiDHHl1MjHHxXawefq1ircA%2Bn5Kj3g6CbQgF1DPrsFrnsFcqT9sVoVtqsXhDQaYbZHvB7ySYJEUHzjxs7YNN6eiSMzd2wDZ0%2FTKm3DwSftX4pQXI69ytgPo%2BsXRRhDSqMAlSuvpCSWIpJ1EB1hdQvWUpH%2FjxoTrb24XbFxDuPyR0uKtvlFQ5gxHox81KEyOwYSplMJjjPpCsWUb2vg9Vv1Hty3Y0V69NhrRoOJbI8aPqfyKRPrQcwDwT3c7lOu9s3XCxLuWCKF1GVWk5RjUwfDp0DoDO52wXj60PDm4eZqGF9CvIRDGxIPWG5uhhewJCKHZ5dwMihD2oxTPs%2B380Bx9jXUK1ii8aBTYxWkmIxjGC8I4sotgRSqVqprfn3Pfox8%2BBk4h1StfvoYmnLM%2FoDbM6F9Vm2k1qC5tw%2FIAZB0ifY9JosCXJQl1hrqvgtiC09l3OlIvQ0HHXNIajnyeYKtVdouMxwmIBSaUKebRZXYyh87OnG9eUcC6lUWfANyFbH%2BmaZA2F53rVi8zWnjtGl4DocH9Ov5o8pTR5SgOqjZnARo2WB4B0RTSzHuqUfThGTw%2F1NVxQD4OH7n1u8oKi%2FNzCGOkRselRQ223Ybol4wi85DG%2Fcesvuf%2BKZaBCb9dcJ3QCxI%2Fvk6kHyptqmwMM24uccGOqUBFOaS%2FV9PCXD1zRJytDYcc9lt9qk%2F3%2FwyN%2FFrCeHR7x69eGgvc0ItvwVJRcG6uORifkA37x%2BpRXyytfPdVx4tX%2FF0xGD1li8%2FQeizvJYkhWR9Xyhk9X0jPNy93BVSiqT6T7iYkajxRFmPmfK2Bd%2B%2Fyb4dnNnWvbGlgr7CbHuKh%2BwMmFJEPU3d4aqF7AqMsu05m%2B92OtRi24lQVnAzhojLtWLAPSCz&X-Amz-Signature=c0505136a5524474edbd925d558c8e90dbfe719693460616d5bfc2d7e6b0978b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

