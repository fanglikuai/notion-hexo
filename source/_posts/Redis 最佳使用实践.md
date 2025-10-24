---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RW2V5KLL%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF8HkDSDsg4MBjAjemPOmPMZa3lfxgCg%2FHKziEFEoEUgAiAVhwzaJ3v%2BpMs0sp3K8oUU54X0BCOOcvitN1rGBOxAqir%2FAwhmEAAaDDYzNzQyMzE4MzgwNSIMePCxEz5bGRqejgzMKtwDPuX9bpJMF%2Fp2ypDYYZJ4z5SX3muru8xmZMUDlOLTmloViOaRkHUFhUbPPFMP8AsgmssSetwdNZDdybZqScwv7COPtf2pyZ%2BVdOx3lugiEbGZMyLapf7usUZzEWwljzAPivON3cM6iS%2Bl9EpbH6sts84yNVFM6EyJtkRamT2GyYQ0tkuVrjK4hMCyj6gvyZ6RHV2Z33Bqv4REst4athBeLtBRR9DsdupzN4IaF%2Fm9QBI9xAP0ytr1WVi7%2BsNh3mf1fHCua1srd%2BgR5AgalJNWzvpnfaNNJTIvICreYlQF6a4Qw2Szx5xYyZIgNYl%2BrSrSbgruBMFX2NxUHvUawwedIoVES4xgX9ctvxOk0PammRqctrm9rmR0AQIMYYGloCAuVJU8EL7W6emFQHdYga5HYc93umDPXLAtlPgOEgNrDAieaj%2B9ZqKtCIrWg4Oc0pjoafDb6my6qOGJhnG1E%2BNEBWOfwfUkZySMAN95inEsbDRplob75hq0zHFEfscukcXnpydpf1dcfH%2Fx61M60dBabXp%2BZqjmWdi1YCfqez3ysGvT8pMHQRR0UyV4CEmj%2Fu4QUdv4z44P1xfAgDsQ4iuHj1Tti6VEBjfA6dys7OYyf4jvQBBYLt95rXcClCMw8tbvxwY6pgFQpWNQzZBr5cA2YVkjiZgcANZ6%2BMKBEgvTYw%2FtijbUbtrtGa4HtzEMP%2Ff2TTiwmelw41pqm3SdgE%2BJbTQ5K0V%2FYcOKkTim9%2Fququ%2Fnb15wVPupNgB6gf0%2BFtrA0SdzSKVRvYC4o28gO%2B3RU9nyjBxRz2oytH3AWK2AP5SjcoxodMuSPhR4EXmLY3ivm756sG4pDeo0J5QWkJaT6rx9H0Rx4pCEw7hT&X-Amz-Signature=74d6f3b6e8379900e279722f3679a764d9a97eddf16946be049f338fce9c38b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

