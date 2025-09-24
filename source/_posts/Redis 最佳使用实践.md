---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPWPNB4J%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDUt9UNkmlyNKeC3evLyJdtd9LKVhgaRPW255ni%2B%2B6xEwIgLPJzQ8sLzp0cKdWaIECl%2F%2BdBTN%2Fvr1v%2BxWYL0jSk8%2B4q%2FwMIZRAAGgw2Mzc0MjMxODM4MDUiDII8K5N%2FQmcDqyJ%2F%2BSrcA59GxKsMVisWpNUJmnTa2fyWa3PbY5JVeZ%2B%2B9w3FUCCV0rZCyCGJ0ea9KAm8agpAiQCOkHwWKoKWtPf8h7mfgMZEagQUdkWUzTVrO06QdAsi6lOwO%2FSXO19eQn%2BpWI95V8cHx%2B1i3%2Fyb%2F4CXH0lyH8QaJiS404EUEGyaFHU82d%2FLquc1pCan414SBWV0tpBQqt7CKfxR%2Bh5db91ejXs%2B7kckifSuUHmmSm4qQy%2FiwMHc3nxK3dZCRGLHJYdLWjg1QqJMlts080m3RCxFsjGQ9MX97vlAOjVZhsRgS%2FsdcXycy3qr44bfyofasi5j7ztnGgjHfpWd6NxPhytBXqSrmPXxfsl06myNB%2FpwqxYKEpAsi5tQX5NFyndT%2F0%2BZaiwoGil1OoempS%2F%2FalFjEJ73Wm6R8hvOr384D%2F8Ps9SmYlSkDCv2N7TE2DxPj5epIrDESUAHOSdUK8kiGbqSXfBBWLQ%2BtkHwdcu0cpfApG0XMfJIgQSRUXc9vBKpA5eA5oUhLbPxsI9U5%2BPD2H9BFfN9RUJgThTw39SVHIj8vrq%2B7x7FCiGF06Eo2bW968BJg7iWayUzeDhoyUUtkxZP2zk3aMfxOv8O73EBoKXvKD6MgjpNM6SgqZWTNA%2FeYKzrMP%2Bd0cYGOqUBMoyQ%2B4Y6YUKZfPGmw4wD9cDjggFchB%2F5SEWtVidSC7moo4me8jUt4FUS%2BJKTBY8nfsSU0ph9r5t0RONh7KuMoVPlIz7ojQkaOfg%2FA%2FttbUH0jW2FbJ2Q6Se42jy8AZqfYvPvCofkkTXwJpY55MKGZgIU%2F2mghfFmXdX%2FFDybuVvisDh5A%2B4t7xAQPvEg49Y6RsDiXWPGAk66o8uH26e0f%2FZRDS91&X-Amz-Signature=86bbca5d834d579fb524641d16a64228aa2034da3f67415a4569580799e9e5f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

