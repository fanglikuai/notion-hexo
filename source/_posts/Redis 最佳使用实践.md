---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSU4GYTQ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T180040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIQDkIhgdK3gjYlbgG1RtQc%2FZrdrFV%2FUSrUMDV1Ifb%2FMyAwIgfMvrqOQXaTrU5ZTeB1Ge0s%2BTi14tg1KdGKc3n2Q3McYqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFSsoWlFgLrzxMcqUyrcAxGwBbbmMABVt3ixj8VRyLNEWlqLOtadMDogKWOggw5FhYuxtVud9gTFITJzRcvCsZJz8h4PLraigF9WrDPLAGknX13d6z59bDqx1J4AC91kiTiek%2BsSFLnazSQmtTSmpm9b9GJsKdHy9gvsPYY%2Fc2DEkkDFnRkJeBTRSo%2F%2B3Dx8z0b3eP0wz%2F7%2BM3sfCH%2FPvTMUdqY%2Fa9L7nu%2BN%2FoooAlpMp7QzSuG5Qy9bH1nkphFS1KBhjznKW%2B7txiLG0uSWKkhh3xvSWXbiikUEBZK0bM9MJTfox%2BxFKq0OyFYnk5TIIhr0x2MBDbdPMYdAZv1R0ehM3COH4dMDLsNo6Vqje%2Fcl1EcLhPx8W2Pro3BrWJHzkHgNalb6nWO8hZhsNDD0UDRRs1O1L%2FXLARpOaKBzoU0mlD%2B%2FE6mp6MYTSmWvjEPfjSc8lhqgNvMXvYdezy8ZTUHsye%2F1N%2FtRCxLRQjnweFNcuyPZLwF%2F0EiYUXgRLf4ulX34dRYmO%2FCAXoY9fh1woe5%2Ff692%2Btsr5D4Zt4XyYkZFGD%2Fd6Gb3nSkHlCU%2BotLAcWjUAc9TTWXf8KuWGVdzWTdxBwlhyIH3DCSX1HUaBsL6sBN8CKWUXFGALVZa4pFaFjG0gNxs7%2BB2Z%2FaaMJ2cicgGOqUBRTIuUajATqoI7amN7XoqJ7P4qSL1sNqSACB1879VWsNHKXWf%2BnBpL9hXlTo4ROqPcM%2BhPRIUBlivSzH6opw33CsqPWF2ewqDs2Ql6QcwIU9bAJL8B1HpXRUDPJdmYP8CVXzWZWr2b8Q5SVYe6kZ0V1n7Ts6OKCS67eLl9Rs%2FcrbQFBA%2BBBZ4dieo2558cUGxEXsGhx5diUA2WpubPKL2Uh8Uky6x&X-Amz-Signature=69dc610a73c58c27710416406985e2d9b0456e1c249ae0808e5138a33c894e91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

