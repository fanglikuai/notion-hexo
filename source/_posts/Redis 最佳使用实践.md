---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWBJQPWG%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIGioOW%2B7SYIOFsbDp33M3hyvgqkffsNUnMrcAhsOiCAHAiEA2Bwh0PnQQIWQCpkcH0%2FQiCNk1fZtNlhc5OBAh0o9cdoqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDXlK8flbNfg9YwM3yrcA7ETZB1q2mTeNZ2vpavlx69Lbt4D2wI%2BApHgU3B%2Ffv8684Kd2H5Birw2PCuQ5TlYkOAWAlnt40FZBHVEVOz%2BCs8AB5jnDVaOaj8dBewoNuKQjB1tWHCpAqd2wx5QL3FjQGrXsC8H35leabYqpVzd3qtEUpA%2FggbXkv%2Bd1bBM77f%2F0HFzfyO0uxgAOnC1vfZzVAXXCPex4l0h1laWWHOnVNjQCkWM7jo40DsVIZfFGE3eImvvP2X7oWPTEw%2FAPQWLoMob0iSWYaoVF5PPvIpKhWvE3iZ4gEK2wIzCg2H8GOAhVpRsKtk12xD7r7vD1S6SQ6N0ZsNQSh1SIvG7yh8Yi8suu2G61KPOeXrx9bnV97WjfBh%2F3gFLnNrnc%2F3egjQ9iwBm22x8PpZloQoczCsDjaPxsiEEYkbGTjsmbgvMRrLzEFyzybenyqEoNa0daQztXsXEjU%2B1VNPPmQCWDXl%2BqQSYHC7nrql00o3oSxTw6Gu4HGlqxKkC7OqiRdR7%2F6%2BFfYLXSLw7JocSxJ3PCXVydZu%2B0a1oWE4W8wFVUc3Rwo6UWhe1c%2BBj9ujvapJK9KrHYrf7bH9yF0dVnII5FjodrvMe%2BwmeP4%2B%2F7RM4MhV9LupIbnaZYOWdddzf9ZS%2FMKPip8cGOqUBxcVuKsp3LVJPXuVUIceYLGGfhK0GO6q9XhnTW%2BwseqvJOliMOh82s1T2ba%2FW5RaEyBFiX95z7ssdF%2F6Mm4bT9TYueMv2P7h%2BIMgIaLIZAvv8X9vM7kzXbRq9wMTdtfBzwAB0mumR%2B8H63DFsWpbO6SNN3VDWcc3uUyUcrujWI%2B5aSxRaOoTYxxHeajkIkjTrjLOwPWQ2k6YKjqXDJXnPfb4tsxMg&X-Amz-Signature=a62bac942234393fb8cea23f17cb9f0cbde7cf844d85f38461f2e2fca9953d0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

