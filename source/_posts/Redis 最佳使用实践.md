---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQO4QHSK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIFnQb8oLnxhlxvkPXog7umtzy72MSTB0GWuBFcwXF36SAiEA5weTpRpvwDVl9PlU091%2FCLGjYvTD85ZrzS0jdhIk6XkqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIuQR78HjJd5FUcZbircA2EGCoPC9Z1%2BZ9JOojymoVzA%2Bmg6sjTJPR71w1miBUjACiTv3KYMaaN%2FpQM5HXeFLC8WSqui7Yt2P1VHHfBSBjONGboAmT5aWgOcpAw8oqg21GSutEKihcCzLdy0G3QQZD8fpsPQecPT2hZrizpkN0MsiGLrq22MuhJt0ifYDQGZCMikaBoz%2FR4tHye6JpzzMhvRKZCd34ASmWEbeKWImkPpQWsANkDPMeEXy%2Bll2%2Fqod8NhQKcXdJ3USSr2ZdcAo%2FpjvYnNDUJJZkEMtgYq2%2BSIV2XAOTnQUDgYreeY5riJqbvvq%2B%2Fa%2BKrKzYadZrkcR%2FNytRjt5wSTEfenbMRi7jEYToWS4ngRDif8gfRdsKt9hgBP0cwaJQuyXbbRB2gcS5vj%2BAsiiiCp8Ko6FKwwqldhNxR7DzuIlMEv24w2bPnCGgVjkT%2B%2BWyJ7%2FLaNJkT7Fn7n0ce8cK%2BA5%2Bc9NFgD%2FRFBTm5gfCJv2bp6cWPregP0OvbVyaohbAr80ZsgELwEHEKBP%2BzwVSjIX17vtEUWH%2BTcxJKi6V3Jy%2FFL8YSKTAnM%2FFXH3xx8PbUT1d2Rjx1ABSsmtyQdaOIsAycgTJw10HKI%2B8sMbAiJtPdf4JDkTgqEwivVd67vi4tuSSeUMP3Xq8YGOqUB26x1%2FUU4SPP%2BjHqmmbYyUjlFa%2BKb5tkT1VOnoJDBoL4zRDS1gNwGYgzCJKbF1IRvwH6mxo02usNplr9xEs1zdhK7SRxSVeg4imaGLNMOmDvrkThpSCNV6MTBryb36MmH851bA1fmjU%2BWw3rDEIqEPw0bL8iQ1oFiuoCeVhfNoOJoIwZPxn0dYycj43fSP8mMS6m99bfvbqWv8sllM%2FB9zLIdHHjG&X-Amz-Signature=f7282ce897e06c29155be3b24f68419edc5de9ddefe700e2cfc43528415ae9d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

