---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VQXKU3J2%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJIMEYCIQDva5bAS7mEfg90nHdR47SJIhYzkoSl%2F1Q0eTwg%2FyMFMQIhAIDlrEV602NWNk9NT31IkXpLcP2UxAVHV70h1X%2BYQyiaKogECKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw3I3QPehISJsns%2Bh0q3AMVzb0n0NPO6ldTe4s71Hlwdz3fF8CuU2cddbGfnMoVA6M%2BrXqN0Rl%2BzTnBWmsmGgPeaSChFmvQCD7zwTWsjgTUTFJLAGLSJKWKLSyOvFN1y%2F%2FqtWScHVK7dxBDaCUK0HhgLWClQUvJ1Qqfs0sWmKfTznl0vcLJfYV%2BEWQi871EIWuaZdgzxUu%2BYtYuRES07MPz7LefkFX1lSkDfOhQ3yBwr1P8BY4dksLXRnv409M5ke1H6869Nn2gWAlMEBRCFXt4A0hwqKfbc%2F4ECi4iRvVpfXUjLMMRRqt31AwVlhGAX1G9WUmXzZIZzpn3ySMoXR2BZBMYifcUC0Dmx7NCLUoanmDmVJMKbJxkjS0vZmg0aprylQmwBOwA%2Fj6w%2Bv4bnjAoPN%2BarpJ6z9G%2B4YJ%2BW6gj%2BoqukY1OkFpwgqGeByRShksl5JYimXl3oHRMSlJWDhlVLCLN4Pcj0WjurAcxuHJw5ubxeVCvp18eWH51Pm0vDN%2F2Gb2TYQ0L5MCGvaweWJUI9MW%2F1YDSsP9npuAIIYI2z9i9YST023ZvCljcpMGk5KvZo1va%2FnM0fq9cl7Qx0eYI1FvLPM87cZiHT5Il11N6qZ%2FiB3GLwilK%2FSf7XXLfrLsfaVOvPxxEtlwjzjCyod7GBjqkAYgnwMGds3FKAIWQGKhjqOZfHEtO6UDTOktP%2B1OouVJe%2FER3uVzPb2j5b40fIrHxbU%2FBoN2NLqwVmCQX0m8HJRvJIfd1Qm405ooT45P%2BThdzu9ME3Xv0HR8hjpp2qUAM%2B6dARtSggPzo%2BkaH6bhKsH%2BPBvVVqSvQMKZJTs7qTvygQml5qY5jzCL6vISgDNBP6cWbc99%2B%2BzXl8NvD7mkwVVm5C6NA&X-Amz-Signature=a29646e4225b82a8ab332467d1ec76a8b9a672bdd001a4ef39eaefda296d05c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

