---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSHJ64KK%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T120058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJHMEUCIQD%2F6RRxC4Sv2sMFkkcWhFw0527APCJsb6eDIqyeIu2YFQIgLEUBeGHeqVqTO5BbABiFtfJjRXnnJ%2F0%2BwPd8yvIww2Qq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDL%2B7rNz8wD83e%2FHyWSrcA3drm3IrYGW%2BrkEcHh1IBLjn3q%2FU1CCOVSEVaiQAiFusIqiNqJknDRem%2FH4XRPcz41JuI%2BqHsJwOa6HHT%2BG7XtZYau2udOlRec4PTYY%2BQndXwci6Hj7sl4HzC8ETpy1b3RArP5TK0cHHrkTHST5%2F9PVXVwSSWgl5crfCOsuSGmW1qf0ktmt052htMCJE1wDkBPVsQWIH0U4hJ9BFXRcPBnS2V5BieB6USPP1jM0vxsL6KAxp%2B1Mfdqw1GRkt8Rdt4yxzI11n7KKgYC7pTm9izo3JRYtroRkUbzbnbX5buD6Yu42VtKWVeYn1o7FlVht2SwRVmOlPsZoNmNwBJNBHeE%2F%2BPRR1MFRXmGCQTc6TwdwDORa0w181YMQkklxizgLej%2BuTaIhp9aq5yQqudiJ69GFfhF8fP9%2FG9f2ZBTAI%2BEmXLDqcgan8ZRA2%2BtWeAP7qE6DNwPwZ1bHUywlI0vI3hovk5Di639FGTGFtXG9Bgu29xHSJAan%2BTZGSjvfbqSbkY220T%2FNkoaR0%2BiwsZzsQwAfJlOG0CrYi5AAuld0fdU0Um74Pti0igBO73vzz8MJbfoIT8u6IOSyDxRxpXFSNlhvjotSOG0WwZFgJEFzL19Uhwyli1C1FViRkDBtrMIrQ0cgGOqUBZDW0OkQMIImILVwroL3uSw2xSqxuR%2FDyxj5SdsIm%2BUPtbVlUGT4B7T4kxPKIQx8vaZ7JLPKJyYAFSk5twYig2fwBgfmdDeNf9O%2ByeWv0mBm5rDiBiPd9omrnhSgkiWsF8cP1hjC6zMuLQPAMgCIi5ged3mIei3OVq5o6XM%2BVYzjia7a8syQ02GGJZHkRiOEbAP7s11BeecacUX3FgKbsND17H0J7&X-Amz-Signature=6d75b6de7adf69d2f381c16919934ef5c1b8ef34e7776f8d23d01f3837993e3b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

