---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJBM75ZF%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T200047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJHMEUCIQCoE1fNhWBtnWC0JHmQzLoM8S92itYuQzuN6Ofp5ilC4AIgC3s%2FuzvZwZGaVeJa%2FDKAs%2FQsLTuNOPsriL7ARvQGGgIqiAQI%2Ff%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBKCwINty%2Fsgw2xmGCrcAznTi2STrikFVGxLReelVpLUyJLH6NxhBjTJ%2BHEnmJwJnWBSorpkc4m5EBS%2FxL0OEg7T%2B9xC%2FLpLOgGs1Sfxf%2BOdC7c%2Fx5VbgtEqt9w%2FTjvKY6SYIIm5mpoARKeRn%2BATB2lIZSrFQ6uZUHPggMu665zcDQ%2FCrENkNPhPIfGGcDn39NO1prTfhXjWN1np11r%2FwiPrIqQrllBjLC%2F3%2FWMspZ3H%2Fr%2BLndrs5jWaJ2Fjy4ljC4fi%2FsR4z12wC1jvt7DouTOl25jymuhDnAV3sFNdicX6UGwOvKeY9TwgC5n45I8oh7WayLjHI%2FYwSUYOq7rXGS%2BpZGhtVC4aRjl3h%2FuF0eh1UoHV4sK7kxuvdvZ29C7XMYosCi4G3RGZaSEIIWLLBLijS%2FNB9QoxieuQffdM2hU6tCuEj2okaxOUQahcyTpqilp%2BwuwwdGGfEINRVfqgFJ0TYJGAa8iwuFfwQw8zEUM65z38pp0yh0%2FEa1SYh%2B2uZhCKRPsv%2Bz3alFACCn2%2FiDre%2Fp8E1deYFeZ72WAWxwN0FZvGKZMYmTcdIXiqC3kYrdoifmIEcc%2F3VPR9mbV71Ks7mS8qePCdaTHYrkj06jNH2pLrdjRWi0B5xcH7778yom5FfYi3PZc1g1tGMLbh%2FcgGOqUB5lPvMAO9mOE1VtWDG%2BGXTN0zFfnu1S3%2F91liaO9CZWZbPpHqYOKXEVXX1wNOrYQs7rz39OKg8b8RQfHSfrZwzbp5YN9uo4ec81aPhClMIHTRsxXXeK63V%2BxXEla7M6A8QqQUzFNlLFT3NiPy9pTENmNeY%2Bw27%2FTAivmjRCDQN78iZOw0NBUdhiJpHnxtKFlbLynwELYsTgwvmiq121NVAolexkg5&X-Amz-Signature=c9a75bfb9cc93b004828f3d8e9734be0e6325871b2abcbfd7805c856f6622484&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

