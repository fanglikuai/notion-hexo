---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664V4TSD6W%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJGMEQCIAc97A4moXfQVMfclgf6KGyzh%2BtVuz9aPninsolGGb62AiBkmhu9oyZiBncVHhzmtk0gnli9siFHiVXYWPTAeVhViyr%2FAwgEEAAaDDYzNzQyMzE4MzgwNSIM6GEP5ULsQcX%2B5rFHKtwDsaEdr0Lud84HIpCSU21Yy0SNlHhheW1jGFDmswaJ4dwxx%2Ft6wygkg6wpTNJ2ZtexdyXAvTWYNFG49A1Mdj3WxG7tKpkaAVL7doTXUKMpmWRURsREoYNSwIqJ5Gh%2B0pSgzCbT3%2Bm7IdK0zAY0QCcuxHI5FvWGTyXmbkjqTtSdj60t9JKunxKvrq%2FBJ5Q6IE9XBuEpwG6eLNOaRw2uOfl9U7uc5w46d2UZ%2FHlsJcEQHQmVQfw0Bvr0aoLoXV92Z8g3kYERYFZtSfaY%2BUPPn50v1sI3nMszdtSMQQL3hfMb4n5LhIFhD6KYQ7g9Vw1BZfiUKPofl95Ndt62y1R%2BGKwaYvQUs2oe2cZ%2FlVr5rOy32dvk20i8NgUfT3gMiCQpQIatXyJyzHBWS4iHPdLFi7E4IGrApRtgtafN92uY5ej2%2FKWplguacas9QKGhaNYHvL2JYFWnVQhp36V%2Fzm6PcFrdWP91v2EK2hOm4lRbpJR2A1hyNeCRq40QcT32xPplKUaneaf45yrX9YBrrEqO1h26BNEJ8dnTZbznyQ%2FRJt4nb4V5EErPNNPRuo6%2FOPVSNgFazsXyTOKbmi7n6xChQrlPhBd04o6ZJRfQNe3kfBqyDqSMGXBJS1Lze3DLIw4ww63%2FyAY6pgG36JDeINQbfSOZj4HDRXQudz9UZPdANDTykIO5Zx9T2ZmM%2Fbuic%2BH0riBgkNY%2Fj1Idoz5Y%2FG28XeOnkh6tk073fi2gFh78Q9sIxgl76MFY7wa2psovvZYeUW5qJhqNoyWuiZF6oxe9Yun76S5fc9YuhNshBditWEK8hhDT%2F5dvXR7YR9cWTa7fu3yj9PpG1zyOefuipoEBMLhmwaZX55kFQyldy0K9&X-Amz-Signature=9905f020345c9fedc5d2606e5926facd103bbdeb89b6bdeb12ff0aa9c8e28d63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

