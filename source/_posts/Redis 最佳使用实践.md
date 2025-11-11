---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OP7IUXR%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQDJq1vHvIduuIYhvZrkEeUm3FKYyueM4ZVFVqd8mmnlNQIhANFXjgytbinGKHM%2F1nRmbZTM9KWlS8dH0FKkuEJnA4HhKv8DCCQQABoMNjM3NDIzMTgzODA1Igytdhag5JXXWzgJsoYq3AOiRyDK6qJooE8kdqEBSfnC0JtPA0MoT%2FsLBoERjaXrldaH70oU61Ckq2hnSKINGORvaIiNt6kV5gh1LAlN2Q7%2FFa4WSR1ddnD0MtoChPAe9zljPkFORi9kvxPd7LpgZsrW4QvhrLPc6czGaBbPl8gIthBgnUNItr%2BQUN2mK22yYrLDUPjQbv1X3GVO37inxZ8OJli%2BKmyAs4gEvwAWsqo0OcrYdslIC1S3573UZkepjzA8F9EcuJVMII%2FsxX%2FA%2BfyhRHUVIIrlxNhhSI%2FJ%2Fx8F%2BU%2BMMaUWiUNhk6k4AtVbnBzYwyxo9vCaJkJcZ6o3bZVRxVo5prPGdQtTZPMvJKIe7u9P53K2uFWgcFZ8yn4KnH1UpKMnmmpYD1GrR9cKRkXkkChq9tVRtu8O0wa9zvECHQbnXSaO%2Fe8nusQ%2BfFxnEMWrEnoZEknwcInjL%2BTwb5rByt9NtNwD8a1dc0BxVFntf2ghq7jDKDazU3JS%2BFm%2B2bxR4dYHRkBQSozvHLl1GWvCM4zt9w5W5Cu1H0wQKlBIFdyRutwac%2BcAdRAvb8S3dQe1ysQLGmsPkkg3S%2B1OfVkqyfPKwvHX5Me6to1aflB%2BOvrcZh7ORJ09eUgdiPdxSFty%2B0J8q7w4CYWMSjD6%2F83IBjqkActI1%2BI0lNzfrA8hv6Kbb86UCwX%2FQngL34uMqr6rNmQzE8wYLTwWNtbenjWYWnPGNd8DxeuDYew3oZBFcebNYTGg8ozgUoB3adR794G7KtIQub5ZPDGRTbj5k5FWJRsHxTZ9OyVsVmoxg8ze%2FvIsKTva69wa9soid3jii0IYEKazVV0eY5BtplvLEqs4xCMI5B2YqKMGBXltsAOrlW9RHE5lRzkW&X-Amz-Signature=92c48f0c57b8ee2e131dac6339485890a38072d145663a08aea5c14d74f7b02f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

