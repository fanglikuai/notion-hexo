---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLBPEIEE%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQCaYY9dy4ti9WPXGqV6n1sNM2QOfrTJgip6glo2o0Ag5AIgGkPRbLxyo4Kr7wXIOP2mFttY9ANlbknzIFCyH%2BVTZe0q%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDKNwsPhrNPHiL7GcVSrcA5UTJVw7lkJTdXfoChhApMJA4lP73dDPK5td1qn%2FokFhQyYca3Sb5Z755BxdM5ONXRzaIp%2B7qC4%2BZqvBwoP8qlDk52HfB%2FZ%2B9AFn77Y8R%2FI4D3bVUmJj%2BrgRfjx7oNOVBcJoOnusOUhrG3I6nxefZT0AJ70I1EzgLKHAPyziRSlSLdIEyrfGD3j%2Fc6fA8PnyPxqJQCode%2FgrVpfYy9Yde%2F5vQ7TUanzOTXC%2BzHVIX3YnGR9nvCBSlekHv0ISFuPzkseaDc4Oz8P9qPEYX2SmHraVC5KfoJAzunPucgkDgylA%2Fe%2FWno2WhrAes0pBq5Tt%2F20pZQbRY6JFYguXl6J1brgN%2BYy%2FprUrERlutqPCbsOIr2XZKtWa8EoiZ18NL9ftmy2ieiAyz5hHl1Bmpwri7J6u2%2Bsca3b5tfvO8avUs%2BwKFT1j%2BZIhjhW5hqXbArBD1kmEL%2FGv0OnzzKWlR9NxBToupZv3Ks8VGKWbhBSen40OmitkzBhhADrOQJiHxQGXwjUBEmON8oXXWeYnZH5PLD2Q8sPcyceaZWHp59DQWB5Qy%2FnAtXSTolTiRqhyKB2SJjq93gYch98UX4Q5kPrjm1K2QxS7X6od%2BWvJKzMROsZeqi%2BkA3I5yqvzOCdTMK3Z3ccGOqUBQm7OxcAZh1J8opv8BE6Zm0ged%2BJic9md8hphV0%2BZPKNdbaIGKbOZwodArify45guS5o7pCO0GPmneLwciwnagyRxeLO7M9z6VAnLkpjcbJmkqEiRazUk0F7hUwXxTgikoZjLLw3G0tJK0vRGaacYjkcSW1Z8L%2BfiAx%2F5f6%2B6fJK0ekiA3LZSgY%2Fgn60KlwipGAo92qKG%2B7iH%2FKt53DWR8QZTjaQ1&X-Amz-Signature=cf7eccc8b479690809233d3ceda9db41d65d85ad7142888dad5e893940fce1d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

