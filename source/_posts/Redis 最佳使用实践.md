---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632522POX%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBk7mENUkv8k%2B99LF5ud8wfDnI%2FhEAJ3tLpc7dphlvH6AiA7R1aOJq5r7dM4y35V4yp8BVPfS0qI%2B8mDTT9EkFUNFCr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMP9X99%2B4yBZV7ReWmKtwDSODNHSEietHcqeeJpgFBvHKn017YIZWaaqaKfumPh6jvy3h8xb%2B0OlM20kvkLwGfx%2BwcSunI3TMtRyVCKV0Qtze7VNAM1ZloEPHgpeNfv7oV41pkntsHvqN2vTX3fAm0I109mSVCWFkCIe9Y67PKeuC6S8QFRHLs2GXBQKpcFVfB1eQpyEa%2BtiOco%2B6arX5meD9JODObM43AMJJLvBrwIV7RvsxX9L4MVBvilYPQxg4sBh13AtMDR%2FEbKauQ2SJ0UGqMjQeXfj3Gs26S1kNIcaYkVMfi0tZDBDX5cPdpjp54%2FBJQq9yMq6KzssQl%2BrzFylYqau%2FQK3o%2Fbbj0usJw9xz7ESgJ78w7eKF75cliQHZgePsUVewlV9eb0OgfwgrDrncp%2BzmsueV1vJlx10mMcndVys4%2F2XUrnk2SRJd3MmHcBRvoxudxMzAW1h4iKrIURctsbOIdkH6y99U%2FlbVt5fPrW8NvWSi2wSPk7qwcMRQqIR4xY%2F%2BEIBDt%2BLPMebpBaOFNkgrei64Rav6avT%2F57PuY8FgF2s6EZTZybrnIkq4FB5nI4m0WbM1mIfmqIesUNoLwW3%2FfQYF0iDZYx5UfYupBp3OhdQ5fr%2F4k6A%2Fb%2BGd7FSmIR6FxODPUTf8w%2B6mmyAY6pgFRMhMLXM%2FF4uJ7U3oXIaOm4pCMrWKS26DDTFddbN3VoAs%2FnewTrurvIzS27dkj71dwrpns84ugby3W1BtGTeUMbMlRC8FdzCvSsUZGPmdG9vXInyi%2FaGdr9SeWQM0pnac6iZ9AFthj%2FQlBJEYtcrr98Ov0gHPUHewugPWJfjKZVRL5u4w0BJTCoDehBXzgwOTiO2GtmfIkTmgddChGSlxJ373srQTI&X-Amz-Signature=083bd5e446542322e7312bea06e04c6cb597d67a0cdfb5dc289bc4dc8d032b14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

