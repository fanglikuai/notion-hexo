---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WCJDUWUE%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIHQA%2BcH%2BsLWT6RPwxCPQsbAO0VpN8hONrDb1lXiWjh51AiEAwZrjR3N1zMYnGCaBC8RdJDNWCghVH5AB24Se5bctbh8qiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEYaR5SFyKw7%2BWmcpSrcAw%2Bl8%2BuEgmD8ELIR9dTtJgRniiRI0PYgOtgapnts12eYqoVNfD22o2H6HwtWi98SCQtaP566U4sRTY1bMad2gIRpTjeY5KE%2BYv7hUxSKyeuAvB%2BRp%2BvSv4NpzF3Po5eTnORCPjhLJsdrKFEM2T16X1uJASCIASfr6uzVWc5HTlNuy8kH%2BEQsB82%2FryJHN8uOentbWvubJmHHOqIK4q24Y9xAZI9IxJ%2BkK0fXaopagETTKzMs7wEi5AcVOQUDmYnCI%2BjEWNQzRm5pOWYUdwHHSd6s6xSQk5t1ogPrqFbnV9ioBWEwERzsioiUrmaK4yBBICxKJBRbja2n2XbP7d93V%2F13RETRigr74Md%2Bd2q3eMeBIQmn0SeqDwdQAfU68%2FCk5WBjXpCX4lXRBzXeueSHWOgdNu4c%2B8rqnIZ9g%2FZExTkxhSVBpHeLvuPD0MQgZxSE4LiUYfLs80rQq06FWjgIeH7f58NL4%2BGyQwf7WLY%2F39gmyAUjWlCqZ8ShLfxAFToaySpb7rdBSsxnx2wPR7YNLL9ZpsBYVzmSRN3Bry6mgXPJNQjtzzTmod8bWBZfy8c8qpvJMCC3nNAabN4oVOCRWTCtsa47CMWKnXaA%2BMTy5z4DXFclRXSvRbwEw2q0MMyg28YGOqUBnkWlP3IVjXVIKJntC8%2Fb9d3Hrw%2Fu%2BDTnnqib1DACjW53WrmiZQ1GyPYJFzpjp%2FRQGzZd8RjyIBTr2CQT%2BN55hzzu9hAhrnt6wKrzHrp%2BS1e%2FxPSlzRqL9XlsGMXg9ENG%2BnBOe%2F1Usho0Sc6UsE7uFuv2wkGZbpW3v4aLl7FIZ2hkRvCiip3ns%2BzD%2BqS5A%2BA7F4iVlc%2Buza6TJgUTDGwXHwLp4AAS&X-Amz-Signature=0b7567341b22d0d9884a5c216cab74bb31d2ed1ae0a7a1550cdf274bba70f8b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

