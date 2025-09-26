---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXNIA4TK%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJIMEYCIQCZQCVHVLOxubSEWSdr62e7%2BvydJWfE5oX44n5qZI4DlgIhAKrwBF5zTQvrgT%2F15hfwiRL4ysNjOklCknyZgdhHHLDmKogECIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgycS%2B%2BXwVxvWzXwBK0q3AO%2Brwbs5RcSfMtEif0Bi8AfiGnaBs%2Fzn0l1ZFCubMU3XdBqx1ablKik0o915IcXPdFW4o%2Bqk359KDdi5PlJ7hHVuflX9feMD2eogMNMUY0%2F8pAXgIl9ByK6cJZYbQvSjbf8xG%2F4E5K4bjA4ZwY8vT4zmu7s5VKUgJal1gXDUBfl63J9efnbXmoAlwTk85YDQiP5Fz9%2B2I4vSjWuanerOP1HleaKNcmYbLyPKkRKpCTP3E2%2FkqFa6hUVo6BGGMhutOCnhCoPaJwAduVhEE%2BzYAWcd6i6szmp4EiV61J4Ufpfsn2kAX3UtZBQtDYd5Gf5FX8ponbcU5jAzlgDSdWfwf8YVEOr1U3EKRrbJq0K3WDNHp%2BpAsyHsALwnmo7ae0nu%2FB4KHx4yGL%2FbO%2B%2ByhDK9LMHMa1Pzvmne88dJsJNeYwWPjLTOiSkp94vlI8fPwp%2BpIKxmfh%2BpMhSaJUuCTH53lGSKsVH9vIBCo958G0VM%2FCRXny1Bp%2Fm6SdQTpBEHiSImIUa8Eck6N2w3MbQ%2FTm7wNRy1qte9OUd4auRvh3pfnwxuLk%2FJ1cakEwqbYEWXevRlgNNN0IXo1HYvuV0GPVU6Q5JAw4zv5KJVtIwAjtzoXam5kqBMTWiZNGYvqtm8DCI3tnGBjqkAbaZxmUKn1E2sS%2F%2Fa6yxWqqRui2y9Vv4XLy%2BMuDNXb3X%2BvqR02izg5uDbmX1nWvcfPLJ08iYRuIXhKIfhOTPM1qQkj0%2FLtQ810OkMg%2B7qOaERwtvITUGpqEQSHwWLvsYwMvkmYui7xNTWYOYgYKURzB4y979qy8KLhpyDIbGBod9h9YlKwoRw5VkRYFH194bWiIZktWP7Jp8Lc%2B72gk2vcSXNK0H&X-Amz-Signature=d7f15a7566f2ce576b9dda3fca079c02dd04886edc5a333378c57f2186284e00&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

