---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RHBEVX4%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIQD%2FUxjuXU%2BA7SkEcct8n%2Fqnd0cPIZ1doOglF0%2FuThRlQgIgEtEg8N16Gv68CvWQYGlS1V8PUStghYclWgoZqG7QyrYqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA1zyadwdPKM3F7PnSrcA9pZkyu4X799CIWV1tMh4jxWnTG%2F02xqgUQwkQHtPXpv5vgzBl%2BvApUhxEcPhQmNLlFpcc%2B3OA%2FxuhrXeQdCa%2B78DYtMDQ9FlGMxBEEzQzfmdQGx60KNkayroyp%2FfTCiTQrk%2BCuCsAQjMJ%2BTEXB%2BNjroobAenJ5sskXOs6pGfXgPRVmG9%2FBjMNGuRbsBCGcp2vul57xXTlxjDtkHRc3QHDG%2Blcwnl9neV7glkbCbUwPK4eZFEd3vY31jaPDK4b64If5iHuSjZYmD1zOtZFDx9cs4rUOy7YNdcMviqneH1pTyxbNbGfhul%2FKF9WKy%2B5GUdNSZRnKTp8pOPlHNLDAd6drclFqiXEFJsSaf7Gt7Y6X%2B5YXfOxUB%2BgPoqQsby3CZWZ9H9EgnMPqpyztSYVWz4O9qGfCrcuLIYVILpEhf0dL5GJ5DUk848NO3zndYmk4JZvbqmARklOCwrT%2BXDnoXLxndsmo3z2TGYn%2FmcdEh%2BtnJ1a3FvemgkaNCmnCkk04l%2Fz2d%2F3N6MzLfPC%2F0Ji7PgH3%2FMIc082SKiMlq4pcILAdjhamkFy3zJmxJ%2BLeogc9HybCF5qzlrq9NWr0tg%2BgLcs%2BNJH2XrZnvyIrce40HJlRgrsGBMLXfXUZT8apfMMfE7cYGOqUBQSokbUFuQHXFfIkO4pbXDQYKSqJJonU0XJu0jXni4GxyCF6qvX1CJd3gaGa1xq4%2B%2BQCjpxlyiI%2FKwq2MH%2BQUrsMfPaXWfIfw0tGVJ4wJl01Dct0LsbokNhfkZYv1tnwslLB6FClfxxh807ttIuune2m%2FMC1YCgp2yQw7AZB7q16SKXhyT1vpIdOlRQXIO0p0zczB9uC65MFj%2FBS02OHmS5IMrsF2&X-Amz-Signature=1ae8476587df2e57f71e92228c0c417e082c519ae1d46136c06c1a5d1fd63038&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

