---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3LVLBNK%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJGMEQCIHX%2FXFSPLdv31jO2mMpRohfKwMBwLBTvf8gN3Viu1DM7AiAaM724xRdpOwBW3paktxPsBnLqFijfiDVATCjLdMH0oiqIBAjH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbjseqRFs5Co3jr3oKtwD20SxAhMG%2FKidR06AopcF8zf8VJNEO84w1vrulacry54G1cOcGE%2BRgrpncDjIpPWbWTooId9mCvfxDaD4w8oyldCgGcJwYhArPgcHNgpWKLJ7N3IKw1CkVCIs6NWRcM1iux5k%2BiQ2xtB3Zt4yF%2FieO3Cva5DwdWTLSnZnAxjLum1ReqASShLpDr430YZxBco3vYqs5Seut%2Fui9qtf07eDQbeK9%2BKuClhidbxIGLeBel%2BtvqUoqnrW63F%2FT3fxkBg2LrXx33gZYnYWE9LC9FKYgPed3nEVZW5nQwtq6zhoF%2B%2FhbSxjz6fe1P7nwfUkl0CTUTqCjnReBs0EtOozPTnKmi74O%2FfMsF4NBJxtWZss%2FYNG4%2FLusDDEi6Q1jh4aDcg4J2%2B7RURV3HMTi4ymkFYTZ5xopnf%2B6MEBQI8T%2F4aP%2Fe2747DW%2FD5UtqtOUsCJS8S8q8FVJK9gCsaa376xuYgfuiWqUZUQ0KcHJKxgoWS6b0EzVn4D9%2FXlpvhqMB%2B0urtJU7dkIJzsnKeolg33%2BKfpVIsMohRBCbG6x21RQIfp9bdYMfkBwR9iIyJ4KhQ8VZRUGvq%2FPICGEtH7SUgNaKhR1PR9YCoQT%2BhwuWGM7mKILuPFkW2Q%2B%2B6o7Z7GB5Mw19rmxgY6pgG3kcCyJnMRANXD6xf26xkuDUiY%2FB0F%2B0pCc8TqAUA8%2B6SA4aWBPMv1dd5PYxfoA6IGX9SRuCLrAAD0GiU8x%2BgG1LubCGzfwfEWeoDZ3WXGIMO5fSLMQhsuoS%2Bkzcrv6VZ6gr6Y6C%2B8DaLra1%2BtQi6VFzwdaQFclKDU5A8gkIna%2BSqD%2Bqrw1UzZliNGdwTmpX2eRHU%2F54iuAkN3ZksmXV35osSGn%2FGl&X-Amz-Signature=8c3de08391cf0dd71d48a4ca6e9419e8452eb51b636d960e3e15fa9298a4609d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

