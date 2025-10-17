---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPXY2UUL%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCIHcLPlzo37sDsIXsgnZMqhEoAE8K7oiWx%2FO8goAqdoQIgB4BHA8HtcXgzGpayexBtlbYeljjpBkCpQZJiyDYuT2EqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO2vfVj6XA4QFOcOYyrcAwubzM68A8UiK%2Ba%2BcGnyq20kXhfAED%2Fd9CvmW%2FudSyrqukLZpjoY57yejFNyw0dN1MxmcPCcQFeGGYxvoUrX%2F6hfMkt6aEN%2Bkuol3cdIBUeVVrHdRtZcgTlulH94WoTGEpvEV6fiz97cquuEGoMrA4JvHOZ468yRWlD0h9E5XpevSpYsmwg5nW6zZsptO%2BfZWc%2FIWbx5qyUsF3bhmKm1X6K%2F5aQFD7T2f7RDM77rPaEx2Msw6lBLEDKECTp%2BNkxRs5f7K0qIob42S%2BWVC%2FBMfAs8tnZtnhsp7OaO0NRXP589zwyXnQcI5%2B9N4LqVAjCJiFexXjAbtcHCksKsBLs027XST3k94NlkIPnvVCNvTDMHIBBo3vcEQLznDtHpHA3R69fVDKcha8e1FXLlUA7cbSTSNp6ar5rhfQ34KRRpaqL4DvQN230J5%2BJJT6hr2m66qqP%2FoQFiL6iPO3%2F35JCH7H1Rn0ahn3sx5zNmZk%2FZpBy37mey3EZuVQgB65LxIik4EBvqXeWOT83d7jgnVtfwyU5rw7loDXDEpfnyI%2BSG8hTVEwAF2wVAJ%2BIsNg2JvHkTEHKVIdn7SXsc4rI244DfFChvhWTSwGmyPVOvXrrxTgve9JGy%2BFSbr66UmgoMMMDBxscGOqUBkROLlDZH2mjdLQ62NimNC1w%2FHAFnUCtTKezpULT8sNXvdFT72tPvPgfG5WCCrVh1XFtSnO%2F050%2BX1DtvkePHLIdQwe6L15qUpIXVs%2BoPv8u2Xredck%2Fa9XIIuIEEmOs7lLY5CdSJQNZZUAr6Q6wD6Zhhqa8nuUFjmdVKM05Ehh3uAO11waPSgcsi9woH5m7MhkAehljIwWTpN5pvLa0DrpwivPVR&X-Amz-Signature=de9c27e9f990d9ac7b55cf6ff67edccaef9fcb3a99b634325c5f83c9cca2e9b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

