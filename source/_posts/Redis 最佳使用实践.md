---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QOVBHCGX%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T110128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIFDsgBZj8tJCRVP9QuUfLNP49Q3g3McDDVKBudMpIvArAiEAsuhWE1ZIcJE%2FL1lb8%2Fc5jEFMqIueFLSDuhgutaKTPCoqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMxV%2BW5omuWcMo79kircAxKV7udXGFQVdg2ApEzRKwGu9gLSw0Wqdw%2FY%2BUThlCaCSlyeN43z5kW6oKeGgiNfYdd4nuWhqWPjLOOB8sRu095axl22e9mZJoApDIMmLqD9XBIj6%2BB3CjvAVP7d0%2B%2FFMfVbVKZivi35%2Fje0jqepJA1X14gb3LWzhe77AwaDX4misPZeCUst8Rpdvvmh9j2sP%2FjawnTMgJ0oCb3ellmDI8e%2BfOZcDcOLGkeUKQuFgupNOiTL%2BcqJDztMAQEC5hjBdxvmiB8psbO%2Be0mWlgB5fafcJJfGuCs%2FhARyVgFNcJ0aAdY%2BaQu2CH64%2BR%2FlmoKpkIg7ZmSWAdJxgPuzsuaziqyll9dbea1mKVsBAHn15tWgUW9Q7z8EfZu61i%2F3mxWS1Vomku7MbHryxAJWOFvWkneIRydUH%2F%2Bvu8J4udDlRa3U6H2bF0%2Bdd98Ken8tvf3Q0aXnDUdvxhGuW2mEfJVpoqE%2FMwTETVSKnuXaCmNXWyFML2pkAwCjs0n9A%2FWFH8yfGd6tgtQwS0rLMXQ7vumMV6HrhjSXrcSPNpQz%2F%2BsiJ%2BctsB1jBOnRfUquu4hRISbBXgEnWkULmX15la%2BxLg5CFCCLvqFdyeCM3WSNcugJ0KuyHL8LBojPQa%2Flhwa3MPey6cYGOqUBV%2BPlYvAHKPqfv9N5F9YjnVrECggvLZBv%2BWeolXXskpDgUglmG27o5lJgTHWpTBPOOXN38WcLNNkDU6kD06e%2F77MVhPPmYFtyTtUcoJPoVAiSzMSPJCTDjYDh7Kew7XZzVPa3Tsbjkh6SuFEYmMdR0c57bU1%2BwC8OZfY0dxJQo8zDKcM3wwRA8Ewu%2F6BSGXMP%2FvcHsYtwLUbdeuRHjFW%2FzJMDEG6%2B&X-Amz-Signature=a638979b01bb5f8713312b3a273d0cb5e787c1da2b98ee673fb5134d0d4ece1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

