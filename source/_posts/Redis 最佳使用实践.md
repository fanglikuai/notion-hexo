---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VOMX4J4R%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T230047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbRvNpjt46zA7sZo56a47CQkA2qi2vOXdiaO38hr%2BXIgIgZtiSLhB8p99oqNqj%2Bnx%2Fz5oUAG54Cw%2BHvRij00RdT4Qq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDJqLEBt9lgy8LnzIBSrcA%2FmcjTNIyyKlQhrFOSvCKqlc0%2F%2FkjJQUQ%2FPHUl9%2F3RKcI58YS5NzzFJ%2BOMuCaaR35r7NLJjT3ZDi949HTCy%2FuuBpDf%2BnvguxyipR7B8ycAChcO2JsKRg8A0fTorXgwx%2B%2BWgKYSzbcAvGmZEtacUrmJd7%2BKExS3VerPDu0ZYZraWcNNmjCO5%2BUz4cclVPcf0GE2SUpc92EZsWcYi2g0zX%2FD3W8JJVBvegFiMQEJHa0lMOAVE6XadPMJSW6IXX10QAVj8UtIpDtqpUKeuKR3sD2ryhj5MrLSvrTcIHc%2FwK2L%2BwCxBatmFf16gUFQA5bWmUsRZZ7448s3UQeoJp7fM3BpfwcwBzIARXum3hbRd8OsBmn7%2B4UhzHuVN7DMM%2BdV0MJ3GbY1ATz6P7%2Fg8oGd90yexal%2BHf5OAKsEFQ91y1laMA9Tb5DYeeS3WrcWRvKMdVEwAOPI6jx%2BGJDqYaAtskwRh2bB0qAN4pJunlBmPebI1wcE2nqpvjbdPZkJE392DX2grkfB8al3GmJB42LP8TA9CTAEh4fSc0vKvqRBl4EWD8JQG02ru3tLZ1eeLCbAMCNpX7Z9q%2BIYOer%2BskoEvqyoHztiJyYyA6muQkdPHFPQcQgdY2ezAzBfLHGlQyMNm5zMYGOqUBSglh7JV1%2BRlwIIgf3hABloVrBJGsDY8nWNDXfZgXdATFktzL7bFMQn9Ch4bNOiub%2BbTNiV%2Bagf1GfW1zTWrx8ziFz0Pa8BjdxmLO%2Fo0awrfNQzyPQXdvdEyI0n9DxXAZAfhGLJd8HS1Gtu01%2BlmXHTCDCzjqMoa4pH4UdxKZkJXjjytlwUfr2N341cA5Svv5vwp9N61jS3sFgYSIBViQmt5eEDKo&X-Amz-Signature=45db9040c9d5d1bb4431a9f77228ac52c199f7dcabc18844fc9fd3107c2d68a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

