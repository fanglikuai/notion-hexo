---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T6ODTMZI%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T170042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQC8Z1wkaPK2iT2N4o%2FyikoZXUUVl%2FnLDtnAlA32a2tqEAIhAN7snG6DrMnQIj%2Fa%2F%2ByQcnvMZi%2Bww1vvIQcq3RBTCDodKv8DCCEQABoMNjM3NDIzMTgzODA1Igwv6GQ3o7%2Bz%2FfSe8ocq3AP98Qk6iCUakxT1hp5SFbR7w%2B5zLnjMMaY4JCCxgjOl7b0J5vCfngHZ2Gs3iwe2kgzS1ebEBOpefkolp3Zmn5ndwDd8FqdL69LMRRyHqei6ZoJRXPqa0XjOV4MfDNjqcVW%2BgcvN9E%2BhMbW800yBTPQM2JoUtOoeMzNU65cS2yfSlWFy1SgUTfjL01e%2B1YPmX4%2FlHwRhVtH2IxnIxldqNd13TjvtMrNkC%2BfpBWUQ9dsiRi0TxE%2Bx%2FNKNEfgT30fA7fzabzduV0zpnXiiImot0vSvSS%2BlFOyE0uXqCXTHOJMDp6xoPqFxg9SpNdBApH6jyZtnehrMJsqwx18nBeysupbFm4ed4KyWsvRSq%2FFKE4zdn7pETi%2FtXGdT5VlxhHtQA%2BHQGAm7nFU3%2FdrYifICDOnxWTYTsbQCFe0scZtr2N1F%2FPer4mj%2FcoXn7uOdCUhuW%2BDhnGCZNOHlsQ4MlzAiC8Cw7TOMjp%2FTPpuv0NZIkLyZJxG9umIpjknhmF1cWLmmEOBpe3exM7kk%2Bc0R05vr94MQTZqYMJ2I2qGuU6wsH%2BR2d20qY3FVRBkQ1aZuhlm22mMxJqY1Ug8NfzRqtEiwqC%2B1jZkC%2BHG9j8hu5L1GfBYWykVOhaaNhSE2ESqeRjDbwM3IBjqkAa4MVnJUoq9V2toPfn6zbp0ezkQ%2FL2B%2BcXv0vH04CCsYuLtVKfjQIYqlo%2FmDx%2BiKxZSO3tIAiuBYRpk3GhtP4WWWbfxmvYqJmvC0M%2Fo3Euitbxbw6qC5YpRNXDIwWozL8%2BcfM%2FwCfpMdwxI1G%2FbNXflCZOxZDqiooEr5bAKRjdelTPZlX5R4eIzrqdPAb89PvBSYEB2NOvhZabJVqnrCxzo1LtyU&X-Amz-Signature=9cadf59fe0fc9e9ad70367aa6ba91399f418a8f2832562b9c61d06ed464d1d44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

