---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SG6DYVK4%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCU0WKfi3v8KiLLndl5aodNJ0Q3r1z%2FFlNJ0e2jGBUn7AIhAKARVAI3u%2FwrzVhRWdTW3Zmf36hRtZO6Gk011V8xo3BNKv8DCBEQABoMNjM3NDIzMTgzODA1IgyE7I%2B1AOuHkb7LBbIq3ANJ%2Flb2kJs%2FSw3Gev6mtWc2fUKaa4VUEeSu5etAhE1%2BquYEc5tVIAPXu6hCIATUU17oZRmryJ1gvsyVBuxOZ8tzgGacqn4xsciaZoG8lZsdqP%2FAEN8Wm4Ym%2B2JK9ErzWndrmVtIuMrGCabsflZxL5K%2FjCbjyJAU3t%2Blc7Q71OEimOBw6lG77WoU%2FfCJtzCyhK8Gn20qgyM9YTbyMUlMX2sHcDPlzI04JpM4OIvyH1aLivqC%2F6Qc%2BAtKf3%2BRKnejvRht2etBedwa23gHmUxyepglbCl8srYwkHD%2FQuc2U0DJ%2FAW4G0n89%2FPz9CAe0jyfQJn7nkkRVlOidNYSqLWf06Ue9jPMe7FnKLzWCmbAwnDLjcY7WzjpqjT3eR75phV%2FgwF6roNhQFJl8TsIrZxa%2BTXc4tI4FDuVU6nrcHOY6sTeovOMXoooP0DhUkVKuPbWt5aFHp46%2BficF4r5gy96qi6O7ip4EmV5MPIsYdFStGsLFuAgap06QUUrauHZ18iRd4HN5WU8PmuoxpQ7sgZxBFEurCB61rthOgzzBF%2BFYc0tsnLqwWxQ3rC%2B7U%2BGtdDe%2FWq0HdgkDxGZY2APOd28hs6ZYYoysSki9ecgeOer1ytkhQ%2BJP%2FOG3GK2tKn59zD3x5HIBjqkAQeao9WJhrlNCrlmzmpBqExJLE1FHI5c6RWdPmh1AK3SDkv93f7DnFSbV4faxENSNfs8b52dgKXexgdMkBeKHq%2FXElrJpxwpXOKKSjfNS2SFbcyELxJNs47RRLSvsBKmzm%2Bb60gYzSp%2FeK5wwrlh5K3OO6WoPmffvjIYwHq3iSZn2d80%2BLJaF30gvOT3xdylv1s5DR3xS2LcJzyl%2B21PDlHE4TNA&X-Amz-Signature=464f664401b529b1489a0c20f30c316bbb119b3e743e364fde749d4cb65afabd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

