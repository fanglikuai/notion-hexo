---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPJL47F3%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T000055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQC4CqZB9Pl9RR4fLgiN64dXgXZML2BqWqRJRjtWNN5dfQIhAPqvbL1JVRJ4A7mmYvTQbzp7hHwJU9qYHXUO7kggSzB0Kv8DCCEQABoMNjM3NDIzMTgzODA1IgyR6Kk5FNVJKKxSuGMq3ANsDJ%2FgdQ0hmhYyD99OZlpTCtxZExlCyny0KowFbUxlNFUSZDM1KBp1iTxVC5LvZlD5fSgulik%2F%2F8puy2qi7maOCS2l94JTiDlFBY%2FJ31tCpo1V27h3GU273zJw95OvR5TSDeR2sVo%2FF3G5ob%2BMj6lCeyGGTQuOiJDoYhEss2VIkbOSUGDSlRJqVHBExjo42ERe4N4%2FpgAe05qtUcsJxBxNTo0c0GYzdFx4naKuautb7DYBW98A2EZ2FNLpwknMMoOlLrrextw%2Bjo4Mu%2FsphOMEEvgGb1EIiqgNhMvfYKmB2cT8wFaUX6RDzv51ylOiPeDkpEQP4LAB5PurDY2xLhSH1bgcOl5nzzpehH6zL%2F0s1Zcv5VBb7%2FBEC9Z8p3vNbIkAEQlb177iFzCALnEM5nMcJilMMn8%2F6JAq%2FlxxvzYakNFEhfeG8eqfBYMiz%2BhN%2BSsveS6rFxt6fJBOxu8yuCqEdBcg7iXadwSKGqZwptE1RKJgPKmUHdqsN38S%2FUv%2FUyVFABSC2HqYVWD9iE%2BuIYNuGrcBfWQ53%2FClOxSEw%2FoqOAkmhBzGD7jeIfS2FfnVFxlqbj50nS%2FDI%2F0wYrgdXynasTr2KS6wFUD9tJ4tQ%2FpBOsZpHHD4mNWmm4nI%2FzDKt%2BDHBjqkAWXmGPZ2lRcRXJwIDawTNH536qWMEeOpIxa2LiTGtee2yFaQ8FMeUTX%2FZutI5PMrsvDvCOlsCun25XMrzLA4rJUbS%2B6grsWDev6eOog9qiKVq2AGsb%2Bujtso9j3g9vMd3%2BYf0ty3xcIWqRtM9WAo5ccLkN56Z3VtIpBI1e2EqIrrc9wBk8egRaeOKr8tYMOy29WXE4IUFuI6QyItnAX3e12Am22t&X-Amz-Signature=552f7fd26752948c740d39399b355cb36e65b2c26127879667bb7600184bbb16&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

