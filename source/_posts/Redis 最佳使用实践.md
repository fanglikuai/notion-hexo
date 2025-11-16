---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665JY73VBY%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCtxX64Jp3bSSCRinITTKrj47L0zAGI%2Bh%2FmH8YR31AQ5gIgaob7m55XQklpTXIQ7mnQ2xuXNwUom1ygMbOVMfOhlgAqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDEyTsqhmJmSlLNvVyrcA0EZOrYqxCLZ2kqzJsHbF4qYhtG0MJo%2Fnc25t%2BpEm%2B8qdj7jSXrW0cQhuOcvEpWyrwSH9CXDlcBEOuu4Ld2AVkmNFab1kXAi3Wfzpk5JYL41bLVa8s%2BV7%2BSp2vppCj6ThY%2BOFrLMhZogANzNGA1De6di1OwmHzLCS%2FEX4TC03ensXozFcWQwjlAAhnG7ojpRcTFSoYwi%2BGrNHRULLYzcaH%2B%2B5Y5RECnjD0M0W2Csrc9d1JIIQpNlLnyh2afAxgHvg0SF3PE1fYIJF5nldnm52NWNA%2FyC0%2BlRQG3CFLT65IzkEp8w3mgbwzZWrKLS1ti5iL7m7iO8E%2FnTRkKM9ic0phYMzxBZ094ZO%2BxmhQe8EajeoQDzOUcJJ%2B211sPL2Sz1IepcgBYHu2BDkI8sOZH0KduD8TFOyuKixAHzDamH13d4WGg6gEmzMwamRIOhjHvrEmnP3t8FfBk90x6eBcnBQ6l4w73dicDtT%2Fnr49Y96MrYUjm7Uyo8Lq0uAZ44UG%2Be9ZIouhW9cqNE0OktxTUVrZ5aqdwSzeRKNukXA83aCnrx51Kigt5rMbwPsnhwOhrdvzpRVBhTjt2WwyGTyQU016p7ThLo8WU0IzsV79RiZlvABjhSsOcO4vZM%2BZ2MMKj%2B5cgGOqUBT2uZhBFImP6C2HfHz03hLtaHm2l2BMEY502gPTlBzhAvMBBZ5opRR%2BWRK76jrjf9GCWrHajEYsGJLeLwtsOCSbamiFnEV1LfpKNWpsI4YH1mzGvUewJWEXX07Jo6xiCMeEfDnnFRhM7ToQj9QKvhX3GY9ES30zw77sZ0JmK%2B2ZdNfJK4NBVQYuclAhr5lITYlsmbyN4kzPLUFnSejpEWsy4cQ4KU&X-Amz-Signature=d2494518f1c1491d26d39e849434f6fb3b9bb51558bc6334f02fdf902264cd47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

