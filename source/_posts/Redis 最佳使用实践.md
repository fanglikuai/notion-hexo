---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYKDYGUN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQC%2Fp%2FLXtEQ8Pn0vE4n2rAtouTePQQ8WSBYaATAlRkyIaQIhANm09tiqXWMsDSnpmITUcIo5w4uk9%2BQ9wUHcat9dEYXGKv8DCCQQABoMNjM3NDIzMTgzODA1IgytHA4dWze57D0ZlUwq3AOluFP2ZaMhfU%2BqnDF9QgzVSdgA2cH%2BILrL4boOjqBZJyuGiLFJKqjb5oscd%2BPHyVPfYs9tRgPI5AOgvjTvYFz2Tw3ZD5JPKoT9QOX%2Fqc8aEcrv8IWuILL52OesF9kamtjKO8o%2FrQdgbF1nABu3TcuBbBgoo0c9noi6Hzs0AkQvMGhx72JHdnXKOL4qQfmaowR2Itv%2BwblmjgpZgUY%2BOAuQyAAJTgGB7G8XEBxGMFsAU0UjJRPriK8gsa0dcByM3MKpBhIHdxR34lcxkzjZoxaTa1A%2Bz68RPfc0Qdv79t5IzrXZlxcZlk9Kmjof5dVe0Gm2ppLGVYnPqAMkvWXKxoqp0j0VLuRwSvvRHpFO3gI5%2FHQZth39%2BJYcMeOmWxnZ1bMZvCpm5Rw6jQiOlPWFUWUySMZKILsGWejyvogJ8kQ8uTl4A6RX9%2BzzGxYz0cq7w94ZJpC5%2BbbX8u2CzcrSC6wnfztweyvM%2FaYFs73wL9l%2FbX1AEk0kU1Y6lRSF7A10cwxHrepLnHSgyVyiVAkyApmwRdRVOQel4EeS3WU4jqtkqkTr%2BCuVeJNOt1YI9b%2BTcy1SC778ExHdIetmg%2B6oSXPDAi77q6pzFOR7sNfOhuwrcOceIssnTxy3MyjhAzCVoYbJBjqkAd6uLCA8EMYaw%2BKjKjTdMVB6%2Fu4xYr8w95LjH5oItjT0pKKrnGvvsddlFZyic799A4KSrzmffr2Tzph0OpO%2BqVHovKxJcs11t3PIKWAEqwSfG%2B2WKCnC6zqcMuR80DTTdvw6T78LwFtUmhW%2FyfibAlzLX0%2BQmLjk6CrTIJyX0BHacagtYmNEyb6v1X8PWQtiCxcaRoGKZ3YbWGdr9Ke%2FBAm0bGVY&X-Amz-Signature=d62dd349b8199dc2926049e9fcca398cbc39a920ee3188cf8790a682ea718b47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

