---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IW4DX2U%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDRHBoPktg04TKMOXxhmoxpE3vJhZAyv7BQcCb1%2FTnKswIgBxXBgxWio%2FEUbGgtcVC8JAEE3UxfL8CMFNM%2B0dytbZcq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDPnVOl09xV629S7OvCrcA%2F7Zbjj7T29nrIoMbrrxPR7LlH%2B6RrHwJ9fj5O08MT04HLfbbx%2BNokrwZYBD1iCWQpSFeeHHS7jb3sd7ijILPv3u9SJ3gLrg45Hk%2BN8xF7vIgp0kqWcH%2B39L1li6amOkR3azW3Dg2oWlL%2FRQ5koXamy0%2FKBYyIBdq%2FDy3ZXCcKFvpanBEuU6sfHaWrp1Nq6H0uqDDd%2BIHa24qiOIvvwrNzRbh%2Fiq4O%2Bb8Hh7WCoJSXzqNDHa4T9l2%2B8BC7BQEeIweWzG5yphywzbPZquYFg1shQ1l%2FJrHC3z7gXpnO5c5lAWpK73vAoF7xewU07cR2wWppDrdxtPuy3QEuYyI3pKUtD18Gf8FlDm2GTcl1a6IKV7O8mmcx6NSHQCc1aR%2B1XaZrXasqbZdZJlhOmAxTiUTSWhcysbZ%2FL7H3Hsf7T3Vy4l5vOOfT82fojZWe8Zl1cPB4d9qBoiAaLnjmkbCsh%2BkjiPAHmMIb3z8zaBQHVbwvZc9XWd0p9%2FYZpW6wnsZ3EEM9AOEVL3I69yj%2B7phoJnmLsywBGKmtqgNILg%2BLVAdu4krB1oZ2UaeuCZGWWKBYu1KWMv%2F4U9uv99s6XMuqgMrHBN%2FuCoe3OSzNhlXcNC9AoUJ89Ces90NJ7XCpNsMM2elckGOqUBTqhZdEFGPQpK8s0IBGwrG9EJElZUg9R2wTjCoylScNRtXro%2BjNzvXyOKRFWSkUuVT%2ByBpzECp58070eKjs8L8Cp9qWUKdUOCIkQJlqjCrRSTjsEIpQysAzTNyFAZn0wwWgUcAs85eTWgWEYS55RL2pAx8zkhY5wGmWfxbQj%2FgIXQHDpbLxWtDUNlpWJqwCl6WUftOD7hqii9LTcZgNa6zTTz4pq7&X-Amz-Signature=d96b32269eb3d781dc5856e112e43463ae5a3cd7ea1fe3c2d0d8b42a596ff56b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

