---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622DZSCJR%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T060043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIFILltTsQMdHonUYygXtuG%2FcYGqhZtaZMrKI0tToTKLtAiEA0%2FtODN0AnxIpC9wq6Aiy9ij%2FQ0ew5uZc0v%2B0lsnuxZsq%2FwMIJhAAGgw2Mzc0MjMxODM4MDUiDO1PTwXU%2B%2FaHnKsWfCrcA9xpDb20sKiIs3uEVp6Mc5bO%2F6mLIrWGZ193EKsY8Kx58CyWYpbN4V6cpCGIpOZw5FhwmjudMc6Fa0YLwFivYdaVwcGKWUHf8r1VJgbw%2Fp3qxLMoEz%2FPMTcpuO22dCrLF5W8Bb6ixX9sXQnhFZA6NlODejicazpmDSQ6ufu7xYyg8a07E0efkn0NfMIrGr0Dy4wM988uHEPcwEUAWZ9QtAGRB0IIyFhX5OyhnIHOvySr3MNwwuk%2BvGnPfFHx2gpAzdtM6J3xkUfFJthxqkrRBmZeI6BMhhXvbMqPqjUrahKd9DEs7b5uoTXkucKXqFChnl6ghpYEaCGzJCqLkd6EhQy0aO1pp6Ox5KzPB3mzjAHuR%2FsDaWpJ7ZhR6Bsr7NJdNdGBNGWC7OQHCWU9Iso%2BwQ6fVqvMkaB9%2F%2FlffyCHPoTEN%2BFySMocG4t8VJSabdtiFSnU1Ly5RATvaOkZ6KnyjeKRm2yz%2BDAYh%2FcIMaHPOtIk8tyWW8VasOJZd99%2BW7T%2BGceNyuieQ07zrQVph6H8p%2BSxcx4OT4C0vEa2FCTmzbYS5OW5lu%2BIUj4wBrt9el2WWgbpOfiIwM%2BFAE1yzoSXF6QCE1MaHZHov76PYhjwtd9yS7%2FKmq82Dlw%2BLyOSMMCtlsgGOqUBuUSdeK97xXQnvDrT2kDKZQlAGpPe%2FM5lPTq4PFD%2BbEfQjfJDQHsFi%2FifKP2BNXNZVOm3UTuWx%2B5qHSUxCwetR9uTza1A59POTga6nUpawEZM1JK5KzNua8X%2FQay1T%2BsnNc5vIdGeM85y4BnYIVsTGTVHYrzuSgC%2FT%2FU%2FqsolteqqYoaVB55xUmpJLZeUsGOVcyiUargCQrpGiI7DDVGD7Xy94y1b&X-Amz-Signature=9a8f9f50c65ac858f25c71dfd2adff67ce4b5b6827dfd98f6bd78a69ea478f93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

