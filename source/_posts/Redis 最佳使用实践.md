---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46655M3RINL%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJHMEUCIQCxTL8TJxyXf6O9eZzdSiORMr7t97B0u6hm91z2LnQj6gIgJftBNQzLN%2BaKehcb3Y93dg7wzaKC4vSMJZ84l4KQqe4qiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOLxfETd0g9i6hlf2SrcA2DLUx4f2%2FhyNhPUSibX3fJZAj%2FWiYcOEMCvckuRDidURLLyUutHGwgclSos7HlwrbxNNIAuJtDKpgutoYEWbBreVzt26ixcHDC88k%2Bmk5XK3iSy13%2Bi9yPQPjgmqt77C0tSBYOVi%2FO8AjJep9%2BsCQo%2FMqz47sWWRTh4HZsO6HpPrAFL2hSDZL9XLO4Q%2FufzxoDOdTOj%2Fo7Dhssz5LsX2lQaaUzEVakYCqj8WvCl0A8R5pz788simeVp6gpDAc7X4qriv%2FF8YvBXRaut1XVZwqkzYjlVN3Tc1QMuAPUTKA1atsMYBf1ZNcd%2BaxVRgJTs4iWhK58yT37X%2F5hayVyKTpRi7tt6mq%2BTxbtRC8wil4i6S%2B4lAkJD%2FFwvdgb60Wkk%2FbkF7jgIrfGb1Xzgv%2FC1PpwLAG2Kax0LZOIx2tLNuOiKsbYdqYVkKO2fSo3n5D7bmmQ7zYJd%2BXLggUYPhkmIpzC6kfbyQJy1OgDI9pGQ3ehkO8A63ewRMajbmZco2eWEr9PW3gh%2BpOso%2BFW6XQC5h%2FeT4YIgNSmxKklQsrxUojd%2FbPbrUHENFLECSZEHcZCYRrvKe50jBgRk6lvO6QhLiSH6z5gddbmvpMKLcd62WAl04cFqVndDwIZJM3BuMP6zyscGOqUB6tYgrZXfc0dNJrwMuGZHRodRU%2Ff6xIhgjP4YDTj0yCIWaAw8iSjj2E6UUfeL6ml6N6wAvfSA3LhdcARbehGmIchh7T2knwkHgHtfUANLzhE1eHR9gW0dOD50RsWAq1%2FfMwA16yggOzkRUM25vTJKPm0W7gntuQ4l4LN3H%2BGX5VfQwZ6xEKOLXM84HtlV4wjq0CbbleiEMkuSNqPOylyEYEmCibJ2&X-Amz-Signature=e0762a1bee516c2a81e49c5e2b3b874006e795c39c2205cad034ed386497684e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

