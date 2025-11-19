---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDUQEJ5I%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIBnwb9e6NFMGkruwcDSln3xrEaX5wxazyGtfY8jP0cauAiB9QA58BHmyGft1pQRam1uRCOC8bIVq4gh9kfwdBnVV3SqIBAjg%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrMa2%2BvqhzGuoN6yDKtwDJCDcgvVGpZPOsk2nwKXmvKYSnuiV7efjUQ1WD0jCzV7PQT2J8WctsnemrwwPTYF9QzSMGw8B6tnYgwKxfN%2BvQotFIj6H5nOBlr%2FYtA%2Fe5SgAN3ZL1szLJK1FFjEGN%2B7hJBRgHVFI8lVdNL6U9x82lixarYQznO%2BpQq4u4wqJNvN9nd%2BEgKNq9eMTwj9sD3KGTRWw404fvXFnuUQ%2Fqw3R2w9bDnxX6BF4eshDCak5AT%2FOxJuonhBEmQ0Q9ofJBoDRoYw5%2BCISYQ0peX0BF%2F4RJ8zTqgonYAw8Dnnd27DD03YL7tYkjitXFcg8hHIxEexZDmbeYox42q%2B7OUqqZ8ALpeoqAzh%2FbXustzmevjTTOSouwKz%2B19AfDHLtdJo9B90EqGrrvjS%2BiHgB%2Bgjqfm7eqvcqST5%2FIfwOqV12gCHovHDyIMRMC59mjSoZBYGSl8wOMBGRcD8zimPrklfjIQ%2FFr0JpHGftirhGLMnHmOm9cgn2Xp5TG4%2BTZ9hXiEvoF6NstpHCMjUOq5hJoMfdBkcQhoCn9%2F3kRsW2v8jpDEbV4vfWv4gWa9NpKSsslrANeaa7As0TO7yQ6xyqLd6Fn2iWBrMJ60C2pdddbScAKP6ayd%2B%2F4gZ8SpIPafy9V84wj733yAY6pgFJlsvy6eKDgxl0r8GySp6GYRKs64ad3fgUhDIxkBkAUssgqWv8b3Zt7M2j5oPieOAGyCdA6O5ZgfnvjDBn8OHWbvitEMVT67mdhqveOoljRtpsozBuyH3u5t4xKLM4FxW4ABOrTJ0GSq%2BYqXaTNGT6YgPWySNpVlmzF20YXakU1s%2BCmCJh405G5mX7onMAdk3yYAaQfprePhanJJTqLBZblKyiCTAS&X-Amz-Signature=45a4f561b98a64ba9607534ff9e2e070c935c79032ab90aa790bc72122565671&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

