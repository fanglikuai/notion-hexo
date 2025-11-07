---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHWJLMVI%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG%2BBSz0ol7MGc8B9cWS2MuKBFd3WSxfmnfMRWL5%2F7UU8AiANYoJDNyaKM5ociOr5tHFjfm4GkL8GhE%2BvYXyhu1P85CqIBAi2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMObCQNHiI6Ms%2Brt5SKtwDyv80eTkhXkrtPQZocEjHPRvc1Zo8ATbPUL%2F2ILC14OhFi%2B23tGx1%2Bv9XmhzJhmFqMuwBiaXhqspcMlKWqiKZDpb9zMiRPktstjg4AIVaxUxuH0knW6jNYuZ63Ho7qQnOIpT%2FbHrODszHik8lYRAUWR%2BqxezJiGP9g%2FRf%2F4R3ml9jIBr7FmDaQb20ZTKnlCNN8Z9gFQSqlz7nGxVdOdCRaweEpMX0pewwTEJCYpnTA%2BPXDFv7j5Lsc%2FHUy2L9JlLQrpoJQqE5L9vJMRC7%2BplfrbyEEYqCBUttRu%2FZ7vprC083foE5anFdwiFhD%2B1hFliXlyA3krLaOET2nq2xL1a9scaPo9N8zvInzzA9RjeLqX5HcX80kp2oatOXMvlCHUSxZQ4q7QtTuxBArr15FCoMQ8VGyVHDXYbaIq6Ig70VcVmeW6DytTRenyUUPSCpBHzc%2BCMR19hiWSMcRyZdtkQht78aQh4nGlUEDKODzn9YsaOWumZwYC1i%2FgJnrGHJhdwRf329SX7tLxDF9V2siaUEKolLPM3STiEN7phTU0pBJGLwZmGc96Z1j5cUbT70f543jypeV712PlWKFo7Hhhei6Cyui2AQFHbt59c8fexTHGiNrU3sGpYwy1YFZmww4Pq1yAY6pgHJbsCUa4QUMdJSQpgNMmesCvhfPLpURqrB%2Fc7t5mmbXiSOentFsZiDfu2mh69aMiPtZI7B%2FDZSvyVqNCkDVTPYOXb3yvNcDMDlBVslgvQnKy%2B9uNQg2SFgjQQDORTs2jps94ECQCZdxHy6XCGmbUtU88DaTe80KYqvZ3VcAYmeDSqOdZLeXLW%2BxozvytuSuLp0CBxa8AALaT6OXLTdF9TcdALzQAIV&X-Amz-Signature=772a8644f46390dbe3b8f51087150888d997ccf84def1f8a24fcccefd518740a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

