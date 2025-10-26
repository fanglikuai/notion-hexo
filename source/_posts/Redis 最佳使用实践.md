---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YGYGFNKE%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGnkwloeSrw3EmIlZw6p%2FiGa4gMItg3Z3HudGw1FT0ihAiA7GTye11CdZ78pRjs3x0YaRCVG8PNEn7yO%2BnAJ5q81KSqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMtWp92HYlnOiVBVbtKtwD0jpvCx6gJ6e3sk2qAssX8JvRMnnDF6%2BIStULBHzOl32er41FbFyVYUkca4HGqLWDGQCu%2FSh96d3CIMAYYilSoQiZX4W3iTXjXc3IeN8kJkk59xd%2BZ7GVpt0N%2FeLBSKhmzCZNZ3VVZ5PdSysog%2BULHg3%2F6AofINpdLcv3RbH7sN0o6FmRnGg2Z7858buPRTDNAmZzsfO%2FOEN4hPeCf3XldhAMjGjcxAUdpuX48jR%2BAwJIkOY4SE4wTADQc9wDupX%2Bxw%2FsNiJsOx0aR%2B11AKim5WczUbG0LU3bYCVYXr5gF6ahmO9bPUQSqzzcYPBHV5fQi9ool171ATKxrjVL%2BJI86JophzPElkX%2BuFSRq2ETtzB7oSazzCE%2B%2FgkIO1MsrUU2lB5C%2F1P%2B%2F3H0px7fiqFugItss3NaDJWNy4JvXNoWVFRs%2FE3nZ36ZwbEW4GWI%2F%2BIt4UjS%2BEc6b1ec23cHbKOdPNZecbUY%2FgPBpEfd44OtEdiYkpf6lxmpj7e1a3Ed929WE5Ig7glyVIbSnlzNS1E08pOusr6%2BPsfatwOfGwrky%2FyIbESNH5MxTp%2BGIfUNMV%2FWq59sChN8IQs0oTFmRZ3GYAdN5mX%2FvYszBYQgrvBjZ7FhPTsOhucipbDzJ0QwhID3xwY6pgFPUZUiZoILSRqi1tUWX%2F4LMZBVFxHXx2PxdtJntyn2ibTTO8gm4zzSrhAuR6UNuuurtmg%2BwiOIARh0hpkFZZXvpG8ZMg%2Be%2FASDclO1pms%2BXOcv%2F6hHAibFh1a05Ma7WzdtfQNx7TlVY7aL9ITkBbY3Eq0vEU3IhFLZUXk2g32IjClag%2BEIzkhWV%2B5idbIsjeI9yuAcg3JIcEfFdao0OKr6zJumkj8t&X-Amz-Signature=41b5d54fa32aaa6b8648ffe045bb3a828c54a72564ad3956dc23a0fd5e26ed1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

