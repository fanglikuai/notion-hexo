---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UW3KPG24%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T020057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICMdzFICoZIYFCnD8TvTQ4Zn0L7OxafwLze0b8WT0keUAiEAublpNZXwCea3DNS8pQMARoGQHYUZ9IgP7hvWw%2F%2FrHaAq%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDLrE2N1fG%2BODkvuSxSrcAyX1%2BLdDFa3SoWGU7iQ19XjgIGYyCdUh0D598vHrjMwNmfBGSybkNUBeEVwLd8UMR0kiDG26KK0Lq82oYraDaJ56qqJUu7J%2FA8En5qxcIRyNPkFd52qh9cHQ6c7n1DsZ4wDM3OojEqViLoCo6qV%2BFoCYd3Fej2HX4mnx96EfFGv%2BArGBcn9jXYnNcY4Wkr3DzGpQC1%2FFgu0Q8vDCJxk%2BynOPbIDS%2BeVr4pWANKrEI70KKZIhYmpePDhJu4XjbEkhLLj%2B5lJCd0MQoVQbH2Od%2FF%2F9NKjamlA%2FWbdTzhUCN7Ufuh04r5SqcvXOvjLTsX6m79CkgtJttt9TYT9u28qKOTTKQ%2FgY5%2BlSnBIvqfiRaix1xz%2FZUy%2FFJ2jjiEek%2BaySvlGGlz00nq77iJcxOhppuPki7Jd9M%2BFSlTL69aqr0VHRihEcJ68jvl3yMGzjaJXoJKlJsjGkM2lfgfNiOQkM0X1oUsPff3fKceV0huWRTGC9k7xiG7TePSd81O472PpdlxMNIeD5fFfH2pTAo3fZAZtxctl04v6C5IGQydw0wZykaipoZfQjcrJ%2FHqC8wFdjo6XYdXbwgV0RNVGaXeecLQIFkkHk%2FBwjGwv%2F78fpZDL2t%2FXCiMF0DpwGPiuSMKXKjskGOqUBrKz36RsVIKKIIr0LwBlSSIX0a8GEy2NvixDEJSF%2FdnfzUVVrIuFb00mugUIt4YcCTk9xjVN8RzlNrrhVKOgJaOdj%2BwpzLC0I552OJd8Hb92UwlTKz2LhnZgKYjk2rR4vYZ4T0xoMoW1J2YhIH%2B8GLKl9xfuDP2M5o%2B3%2B%2Fcr7kZHp2h1qCzNLgi1l0wZy%2Bcv8p64WJS9RSKLxLR5xGL3Nqo7HRrNO&X-Amz-Signature=b690c82358a41d52db1a04d336258252d5eb46f5e505863c4665f89f63b61467&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

