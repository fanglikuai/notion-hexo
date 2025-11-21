---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CQ3B4NV%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T180053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIQDP52q9PNTe8zWZ6dpYR45PtV8vTX%2BxEDhGsbHizYgpjAIgGqvlYnkH18hPXaOpjkt%2FCvjJ0kaOG4%2FUi%2BtIPJPSnHEq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDLutctyKGLIP0xFxFCrcA%2B2SWAIqoZE43qq0hh9bKKMSTf%2FgWElERQ8PBsvGiWm389ztF%2BxcOIJM6xFw%2B5Uhgax9wOqtvgxTu6kerq59uzNNwN0NtGBguojEABx3KeHbocAkI5EkZshfPIXrhG%2BQKK5URFJNlbuU9PytANlHbysUDwztaaTKeVqccJ7WJ8WxQ0IGPtxjlAK9bZPG%2Fa7xMk8VFMdMHxwrOORQtIVCPwI0keXrJtIo6D5X6VsHI5ts23tY%2FySQP7SwVywZC85Vqql%2BCfC1jU7JpoTaROHFfRicEsg%2BiEeSBORo4blCUPTl5XQRHftTSly7fmQfVDN8Q61zWzh0lLNM5RH6O6aziaM%2BaVoZTCbe4GPTAJdWIlQ51A6If0gAD4zMnZarpkQddrAT5%2B80%2BvTDdnrKYNHmzbgnWKvz1Qo2KDyDuST2VlRLbVVKJXt9eP%2FdzZejSOxMPvuW2mHj7kOqZKUbXmAE%2F%2BIorH8c3L6C428lohG4edQnWk%2FnCDu40aGYAfzfFjvXgV2mJEdIJmJFUvzFEk%2FsWYDT%2F8%2B48gBFu2LpRgEkixOxYR4D6LvVXMstgMgbK4rgS5lgIUXRBI2EbcJE2hSxAYVz7vbtATsQa4%2F2IDlOCtP3a3JCR5dtZcNqozs1MIvJgskGOqUBSj%2BqVURJyVXjLhV2RFOf%2F131gcvqQnkQCSr1UVwjW%2BViftDiJSpD7uy%2ByN9BAOMgQ4JcErKe1ZBfKtu8yOzEqli6Ni6kRoQkPxBikgyfVwb%2B4hbK9dbfIi2qbrJJRjUmziVdTq6%2FOOJ%2FctdOlwGAV6uPjgcosXWxaQsMxNVnq%2BUMvQ8fDWw%2BqgDNdHVniu0dy2MxIIY4aG7k42hHswtDNSozdYkh&X-Amz-Signature=1f9745b42d4e550e5c9cafe4f836f5262d12f1b1af651753a6cf63f3cf981194&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

