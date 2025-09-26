---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZJFYMQF%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T230047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQCdKFs%2F2bqvGQl9j%2BBDD8q3wtsEXeb2SPMBR45xvkLgeQIhAIdhV95jnUIO73zZ1urCOn4ICpwWCEkb8f1NoUoz3%2FcCKogECJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwK8LdUBQQi1ncYUVkq3ANWcTUe5sUIwk322n9oM1HGzaL%2FuXl1O0UJsFMM%2Fs10pWGg9D50%2F1OrY6EiFugKVVACa5pxZxYnLoHq0ye5VIwcfDkq%2FY1J9H67Ptw5N%2BNWVLdEnq4ZnO7o0I9YuqURPKNYIrNQ5k1VmW0Ju9Mw4nO7oMZ7%2FZzCnxu4MetmudOkPmyOZnWujINOj6jzkl5XpsDrc2KtYoyq%2BGKc%2B8hLFABK1gZAVl%2Bi0PzOTZBAKatlopcS%2B72g%2BpMFQ6NuBktp3gcovXXz23KMijjfZj8hF%2Ffh7Z2FwwGC3F3OdMJghyicoS2DASKr7ozy%2BVCAIXd%2Fz3v3WtIpukw3U7hGMzT2%2FBH%2BKf54qNoejrMpxDB75Wyn0xJDD%2BSRK7isw62qt3lu0PBMJBYJlyRg%2B9wbIs6X%2BCFZtA%2FtyfTnHcoElebFedezTVeBjZu0HLIjbSkbViKoPqyyGi4kf%2F0VnJ8iBsMlNLs056DShGTZ876BFtMfwSsRpXc1TipGLSa9VXmPNnIwFCIgmtIaNCxpwc5h1P0qh1Lanjwd2n1ihuj8hZsOd9%2FSTDRyaz9inTgC3oeYIz4BsjjnKtL0LPnB0PwI2agtXl7Mg9qjZ22elg%2FmVuldG0GbvrQcOWi4mDHlap8KIzDWstzGBjqkAW6qKCw1d6dHm7udEYB4LoRuDKhX4rveauWNhGviqUF0hGS6wr%2FC6kS52LPSs2s9d7rdQ9Y0yApGEtmEDCZQgy9QQ1yAo%2BdfAuT4tkF9jIRQDEPqD6lhdUY1PbZdNqWC4Iped%2BEvuK4wvfDQw2mGBka64JqMORIizknmINwslnbw%2FGAq7F5HYfVxLAMNBVtjfdkD0%2FVsGObEdUbtqTKO73M8uz3c&X-Amz-Signature=f8d84fa25a87bda008051b0ffc86641883ec04f23a22e1ffa023b21d3d31da85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

