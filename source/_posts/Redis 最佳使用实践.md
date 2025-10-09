---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZM3CLCK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T140118Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJIMEYCIQDGipmxiw7Osvs%2F2GUzmQcYllV8RhKe6wlw2PaH2CFtGgIhAJIZVDMqyEQANFjeWv1yf4w32S9ssFZEt%2FsfNEK1lrKqKogECNb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz8em0mz65Bjnr3QDcq3APYJrepqERYaVUAuFwKtkXfMHGbrJMxT6e5ADaQbdjE%2F6bijmWgnbQ1hVrVdKcL4lxr1AM4u3dlXJ3JJgCoU814pnUPX0D%2BlE8LVmvK5jaTnPEs6msdWV4PNlxXj%2FtxxmDOjcjXj8pmxQX7e2jp5CXHqTCa5PASNnJRzA9Ap9VDekYa3F8x4gdrHrUO5alhebZ%2BpD%2BiNzkahbKtk6u2Jnlc9IyD%2F%2BQIm%2BOxTMhM2lLQ5%2BTB397PVgAV4eLg3wKzzJjlJJwlIiajRpNR2Klk%2FypegVBB5n8uc1JuwjdgZZYjUqPBvVszpt6oE7Lr3oT81lUDP0dFo27Lv%2B5rCSL34ol9ouuNF7ISsbBuVOS3wmCTg80syfLQAgtHhMiywtKJhPyBckcRWKHqoAj%2BbKvEPBVw4I5dAq9qvsSGAH%2BC%2F0woG5HeCphi0pL%2F5T2xokFBzNHBTrVLnaNaWCxkTbDGTtaIffLedenrAZ3tBv0%2BfC1tmYHXHGfMeHBRJv4qwPwx%2FfrSvp1PXQNa9eyvr0tlblv5BZicv6Q7LNKixBdMRhY2hHFzUlfcUW1mTT4u%2FvD48lr48eKxpntaT8VOaXqT92x1lcuZkWjd1aVwXDCqJjk2rS9x5vJyxFBy8zDP%2BzDL6Z7HBjqkAT1mf8taAoSI5kZ%2B7CIgE9YMqUBmJ5oO8ZmNTgd5IkyC4lzvCCNg%2Ffmm%2F76hDaLMLrp3VOdlcR5PvtCF8aWO9XpElALxrSuUVrcOnkT4L%2FCHnjbLmd%2B9M2O6DNPQ%2BgNpcgEseFnM6MqRv68W8jojIBOBOcAx85R0py99yBV8%2Fd0rY3K73Q1ezg%2FcxuLKKCpY0ObZNoL%2Fy4Jo7NY8x9fxSDZjK2QR&X-Amz-Signature=936eb0b6dde851eaf4afe640b077affd28ca230a89c29e76f71e762904e683ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

