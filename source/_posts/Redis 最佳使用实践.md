---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZ2CUTRJ%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T130102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1f7E31%2F7pweX222zcFS%2BwcO0XUqV67GfLXZqUfEvWKwIhALFbiz2k4da6Z9ovxVPmOM7IUY%2BaChPbxYbyJv2Rewk9Kv8DCEUQABoMNjM3NDIzMTgzODA1IgwpFYSXhgQW1Sqel8cq3AMBEUwb6VKFMhJydfXLM6tkkp2iDXnYsp1TqXYRVvejxvYQHt%2BC0FrCIpCYD%2FgjgpaRZjtrut73dJXaNcBaq37h9g63TKvTmDmo3QeRiQc2ruEB5b0S7HUarz2G9wlLLq3Z3nn6HFHTsLjnuR%2BzUUc0w32B%2B3xN8nJI9pkWAOU5F35fpFt5luY9yymTcv0GIUnAuo01LZa683jNQHt7wD81ysMGPm1MuJGwBKIZqenkKCim2%2Fjl0dOZYVTJH%2FhA697rEfzbUS27KYOLJTHLbk4TeYa0jmxkuA4Yq%2B2mLxFFjsyShQ5x%2FpFJ3AZzojIleqCH8eIh04ATNYGfDmoMRgQ1%2BJ3U61EDHvOwYs61a2zS1Hybtca6ZpVTWTyVfZaQ7Sr%2FlheQOAfCS0dkoytpvrO7TspDxIsWsJmNlX1y3Wk0I1UXKl8bnndZKo3e9Z%2BlL5IDOv7klGTzaoNXWIhd8ZLmLzhv3XwvATiEPAPwx46Y4d1OQvVhsiIlbSSyl8uz3eyKRBHE2c0VBa70NkEWs1t0mrqZ5V6uoJCyh3KnsH0ocVyuX6hzH7F6llFdDbDBfF59%2B3Z0XBR%2Fc8EQG03ac5%2BSlvCvh%2Bjx%2Ba2zMp1mKiaWi3%2FsgMmGZsKUE1XsbzDStujHBjqkAd4jEaGPynx4g1lcl%2Bg02GjngMdwzjXTLT7KEHXC7D%2FvCrMZzdSQBe9uZiA8Fa3oss3m5F5Wnrncki0oq4ZUFsPSRNd%2B0eS5qIqHXxhSC71HS6fQWv8P3dg7suZr5krZdmo24G3AZR%2FgHL21RELAdELuueYBuDGznfxKFKvzg7y0z9v2ax7e2o9hFp%2BrX16d8nDfdBkesz7dhfTF1cDJPlXo3rCV&X-Amz-Signature=f0bd97823977fb6343007495d82975bd1149abbf37b5731f17ea1874ad316b20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

