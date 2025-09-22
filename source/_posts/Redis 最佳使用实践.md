---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662MUZQGHQ%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T110038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDBjgCp84ZQ4AkqnHFAZdw2QBCdbkazOPuLtg2uIOzMQwIhAMk3Qb8aFoh3jVsQ4NPlluNB7HTjJqynxhvB2m8FM16LKv8DCCwQABoMNjM3NDIzMTgzODA1Igxnyi8TpGvqwznrWdUq3AMYwD6O673v1Z9112R59JXzMtAsp%2BahtoM6GQvQBgLynEgRnOkMgFtGRLFv2pWCFsTkHuX4hh1Z9ScS8yD8YFE1ve%2F1Fe3esMBMMmvbydwjwT7%2BuRVyi%2BDc3s3Ou1Iw3KLZzeX4m8QV2toEBhyFNQm8K1aUeBLprJeUnUIZYZjBW1U3I8MW6icn%2FRKYTFHfB1JXaSobCn%2BeJuguRj8aUcVsJIl1xD%2F3r4nKWDL41o5KEBnQBxhjbT9dSLYZgiDwoON8JgUOz6KcheDMHNwcBGu0tDZzEQr%2Fg31moRW%2FR1pv5MucMPdHVaEZmg2KS9c7e0jBCL8OJ5SaFgahs7fMJ4IiOv0H9VTTS1AnxilXBan%2BMLLZ%2FzEC%2BSTh5Pa1ex57Ck9RD1twKXrYcB%2FF4tI31%2FF7Uky6U6k4S1QTDvXCKyNHy4QdgXRX3K6517TSjUfSo741kFMEk%2BC1TkihDsma7hyZ2py7ThyMqVFwu5Ego%2FbaLddiYoxntrYh351Y134%2FCfnDiGCZ51sWda3NhZo%2BKWoQu5ek6bsrMZceHL8O%2F7fYe6Lct76CoCTUu%2F%2BAbxkfTNuk2fByJfFyqTTpA4qvXa7dAs800Rag3ZMlk9IAvmm%2F5L3LNItvHUXqO79v4zCu0sTGBjqkAa5Xx%2BAgsFj0NI7CR8nED9FYtKg%2Fv92MYDP9H%2BB2Xb61sMTFz94t0f9wVbm3nzzhmx%2Fibp9cksyOQL3EiaUWy0U51Aenmzl48JsvQS5nMFcF0WbPPmCcC%2F3kzi7zUUzElmc%2F5YkTbzicV54HNxs0RAdw6WxLuwd36qNKYav1QEdQP2%2FYhk07SA%2Fqbimd82XKXQ9oofBE6emPXnyO8cfjn3k6tj20&X-Amz-Signature=00cdf0e1eea61b90106ad59816c0aea68d80dcbb9c15b2db91692fb5b298957a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

