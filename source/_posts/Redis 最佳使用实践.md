---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLHXQEZP%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIBC4iDynz3tTV8HF3ySumZWDi48goJW8WVFTtZoz9tNvAiEA3FuFF4dUP%2F2yCSBJNmqdJpPDk9rVvBYmFh0b8Hlq2Ugq%2FwMIRBAAGgw2Mzc0MjMxODM4MDUiDGDa2TG4Dodi1qXiKyrcA1BXVwrBlZSSUnuk3JhOktqpNoxEKvzxwX4ZUGqId9InGQ74sG8aHJr362X6%2FurIWnXv4oSxU7IhyGS9EGRcDFgu0U%2BAVuSkB9Yr3z216OwQDBnvh%2F32DGb5l0zswDTLqGj4IP7KXIMtC5ikmc969gjPQzVKPAvUjeJ6hBl0ey5vyQV9HsgmZErVevQij0t%2F%2FTHOBn6XjLEVRJhxd31uTS4gTkZJuARqTnWQw1waJi4p5NWt7arwf1MBAjVH6VFvwPrFz954REZUowKkXaqzUcpbarptKGR%2Fl%2FVa5jLnnS0uDn4HoYp1IDXjPRRUyWIf9rY5RaVGvTQkntV77kLrlW6%2BaGKY7MKOOSbLq8wocIFRXBoCwJdGM3Vcju3Caq7OSvwi8%2FZHZvwPwsZgmCSl0pxQXa4S%2BmhM5QMsHgcY8LOrzZLDUZWhXGBkNHZAzPkc872GdYTBMxWQ4Onb7H%2F4x3hGANB15gPs7Y9rkl41YkNSFde9rEt2UZtVubP7bDluvL2OwMJDfK4aJLNqutcp0%2Fys1h%2FBI61ONk%2BfX3RUpeSzZXOaJLl7fhFUAQqvWIYaA%2Fx7mCn%2B%2Bn%2BXyGn0DqJA2bZj2kVl%2BvkAl%2FC35LdcpU4dNawD1vjEPl2NlsSqMJec1cgGOqUBZDkGzliIhxUWX8zYZ4bD7jNwglCriUpg%2BNKlEZUfURb9yb9zsnQp2q4OvGRlFzBz6VHwq%2BWiFGRGVNVvDxMddPl74sCY53OYB55ckqn7pPQnGdy4PTUqu4iWCV3%2FTsIpitph2%2BWykQkik%2FGHwXop8lBSQ%2BnBIO1%2B%2BXqzs1G5DGXnXFMNw40qNTMiPe7jLIhgAVs%2FwAUaWp%2FnC8gE4IeOjuRqdREC&X-Amz-Signature=3ce7120fa22837cfc35768b75477c2430cfea16a6c5b6d5bb6ce75add6eb2df9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

