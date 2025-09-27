---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OLJ74EA%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJGMEQCIDdCzzBO2ib3BP2%2FfoAvZ1fXEuP55iLHG1smz67JoBdwAiAUTkafr0uicsaSTvCXI073EijOSsJ1XxJ0mFck9CPBfCqIBAie%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJU6fefmAVMjMr7nxKtwDEgrUn2w4DSvvCwMf7E1H4OqZCGB8KmwaSOSOzdfym0Wbae24CI0R%2Brjy3wSARkZsvwY2VRdfkj%2BfHkKAIFMNSeqLbUBLFn3zwFBKRHzS%2FBdskI0GeNIYtJLGO%2Bb0jG2r5wnAJB1fAci07V8uv5HBC5ddGZ5el5jlp1eKcJuDTv2qW8K%2FkS%2Bbk7QdcWMPUzHrBdeRP7f3D%2B5CMIZq81Z216%2BGhgqg8jVs%2BZvA68nAZRm5MMp8RwX%2FC9SO5qL1G%2Bj32E0the%2F5%2B6namp2prHVzPziAmQ%2F9vLggD4ioiVKS3Y7zLzk0e332lgHR3UlGV30C1Pd7oxbgVJrygrlKdPb%2BWN7N%2FVJ4rhYr4JJ2jSpu7qX%2By0dkMCyQH2EB%2BlsngGuuOhPZv4%2FVS9IoIvvYuhe%2BzgpwkYYhdvvO%2BWd6GZrzxyuNzjzmYfB0hxjhjbthgR6vctp3vhuOFBDvivSAooYIkyzKcESiJ00rHRQ2fYoVAthsh6%2B%2FUcLc4lHRE%2B0HMC%2BeLLhDpbRwkCWtCaooVKi%2BqdSdvXT2yOE0MlH6vFmtriUauF%2FouWHdYaDpdT7IqkwxnChz7yRtzQFBI%2B6HLyNsX5lM6%2BG1Isa5Isf9Rl7PcvMJJHTP%2BCMIZw6w4HcwpNzdxgY6pgFuLBWqN5wTpqaZuBIVEvkygGuwNdUtq8D3fnB5msR%2F67HX52uimOUPEUBQESy7L3Mecjf5LxZXl6GCOtbctZOEbw4l593aJVZdXU9UF2oPbA352zzjlEtkNenOVzs%2BPTN3Nv6WusWQLxJ0xZ%2Fe33ZTwHfzfpmpeWfzF2l32QtJFrmdkbOmATPw9FmOniAGNFpYxw7IWf%2FJpeWSDJRyod2%2FkXbcV1p1&X-Amz-Signature=c13f9ec4c09606034f8f3e7f0c6be7dcb5821a2161d2957b7e801af8330a20af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

