---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7P7MIU2%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T180040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQDevQopGW%2FVTyhyPNukNiBBel1A0%2BEmlWbccgSXaiBOaAIhANGCpD3xi2FKXKjeZqqCJykP5cAbK8%2FrQFiJDa3qbLfwKv8DCDIQABoMNjM3NDIzMTgzODA1IgyQVM4ctXgzY7oPx2sq3ANgU%2BiHy5kq3CtuRjKFtYPtcGMX8Vq3e9vuQXGjEa0IFsZiX03REdeWyBbNL01OHrItVW9BbGOsloiZT7Lq3Nf2soiEkIpGlUUtcwbm0N6e2mQ3zIxaIh6KNX%2BvVxgqt3ixxqppIAgdQXZzRfZxudjDnv2OTJDXaQFbKXpxUuJWkLDAEdwLmFQSqbIXxRfM2gkzJbH9KjWwxXxSXOySKDP%2BJp3dBvjAaxhmyvO8lYErWzBtAWo%2FdE3Ch4sOfynS%2FcDRX7hIFgR86Uw%2Fzp3kzaHENywGuZco6w3cZWdeEkpf%2BJ1HeasCAPw1j6M%2BZ5Kmx%2F0Ah%2BHYH7y2oR8MOtQLWVY74XHU9LkkrZFIxapbf57UDFwFRDFAI%2BLP1Kfdk0MScQibp7KFXGTJKjV6LMhn5f6YvHDoacOgbfUA2Y%2BrBo70WSaHVimrb%2FzqIOz5fwYHsq2p9L6D17fb2JHb1RYLt%2BqI7Wy3eg83aTHs9nj7DIeXPXMrd3OyqnQoCExg73ykbYeH0npw1PFPh5qiS0QbgMzochbSkjL7edEPGOjinrKqzwgCsOLWbHmR0X4qX4bAb51Pf2rcRz5XLhzKP9mzeBl%2BgCO%2FMdbYUjvr6s5gr5zN6C%2BuKT1S7FvJ%2Fm%2BkxTDv%2BJjIBjqkAXyfB7%2FOMFj10wqXS6wPPj26SoeIm0cPBrdRdE0Sa6HPpAhxQELjuZ9OyMMTfeImH0kglqlOGfD%2FfGKQpc3xYZscjEpKiTy0AjJoIo8q5gMr15%2FU9VDaUs1iSkzl9vrdpUluqtWB0ue1zPU3wactlipXzKe1QC7676wSe1lZzmMxeCX2uRDkPxbmhYv8%2BfA9HZmpak3ZlV%2FgCgU0ZH5id%2FkG0yiS&X-Amz-Signature=d66b3efb3dc95d1e8143a897f66b06382cb473392abb3cc638145f9085a527d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

