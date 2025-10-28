---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662C4EVL6T%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGtRgF%2FtYN2%2BLAN2JwDPT9xal1yjIxLs5Vqw50gDcCAAIgSwqXagQLzvs3xPBL%2BHhWPl0N5ZObrlSFmjrxeoye2EAqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNRTty%2FrJj3s9%2B2CCSrcA8HgIToeojlE8ZBS9kA%2FBggsEQ8Vq3D1NzzCfpLhH0mQoe3bJEeaUpqRbhMQDDQ6vk2COYCt3iLxr5PYYM4FchvAVcaLMEP12ZnoO%2FGPsJukcd6Fa5Ld1ItkF7%2F2YTk5bkNmEXpat0fs6gfKEfwDAwaJ%2BE3tkK8nwSe%2FTsCN%2BFomS36sfHuXaNYRh8blBDH6yPv6Lk%2FHbfixSwHcPxpdJPBUysnggfwj%2BoXyViShi9e1sfiwgiE7Z5ZUyFw8OpDxmAEQvQosxo5nTyRSljAq4SV%2BMSwpnjUt8%2BSREpI%2FN0y3D6c1vIirwLemBfUzWHFyVU9VI%2BgRNpGSaTBmN5hgqJLI6ALUJ2RLJ2A5XHSxBSWNIgB965KdPJc5MIw%2FAwB9RvLu%2BtDqeKKntJZNV8viEroFf6ifUuSqesPcBQS8w56OJOdLoYADhhUuxtLgUQYGciIv1zilWuqO7%2BDtvbnp6VW%2BvsPy04zgFQ2eX2dDeylVVZoSay9GvMk6Oj3Jl95%2BJ9m59RmRO6%2FrhRqUTrE8bNqTchaO0TZdUTYVkPAQ673WqLLzs%2B1jYVsWtt40PnCZWEPU%2FORNr5%2BcD%2FhZkzzIJdzdi%2B%2F4OqX5d6rn2h56dwgcjWOp5wdlH9a%2BG%2BVoMKK7gcgGOqUBH9R3Jv6gzKXChmBM2URU4htEs9NabdNts3OZdWZ2m2qpn6vkyoghXuGRzPYG%2FQB8hFbZZRPWPaqutLf1C9Pga6%2BkgdRZLxnQUchqZaLX49fnt43P3zAT3UsK3Ma7FuIfmhqt07jqfe6WZv4nlzEriE1A5HavBeHxoQ%2FbMOdS%2BKa358LVlwAv7qMKZVDRPOQL%2F2yqdxUEJmQ2dRMx%2Fh8Cqzs7WjH%2B&X-Amz-Signature=20fa61522b8ca4f3833a13813b8be6d750318d9fdb0f55773a226cd81b839675&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

