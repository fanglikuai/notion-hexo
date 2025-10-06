---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDBEFVER%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDw49VqDZid%2FY4w3UOalarWnTxo26SKVgc3BEHsPHxSpAiEAz8HjfoH5SvfcEWPIexvC5inV3h0iy9koYU5bzfbs7AcqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDfvXPFXAL0hOt2pZyrcA7HLCi0B6IJPRtXLBQSGHRoiVY7nCN57fWoeBVyEXW8bxN%2BgurPMC51Rst%2FH8DZ%2BIC6GJgMUvuY3TnO8yokQPhQdb1qLVU1xSU%2BnBEyMX4tEqdUw5xrdr9VaRkTyrjWDfLX9u%2B2ooH0%2Fdw8IPLBpB1%2BtylH38GAUXTKm8qVuXd4fOqvdPIqcQqYMa3z2d%2BaeumYERZtUzphElrBzQH3ohmz1zPl4jYTgz72EL3FrPPZC1vzi%2BZAtw%2F955bFvvx%2Fp4pXp6BGAdnPRYPa3zq6PgoJ2MHCQ39SM6Z34zo%2BECjoSF26l11Bx3kcbYuSs1R%2BgpUeHiGzvWe6mJipWusCy2TmdjkAMm0WS3i9nrhrbe7I8rvOKUUx0qIeXL02hyEG2EJwIAIPNyeAFFKATDq3ByCs2fisjz0sHod9UKTd4C1roGmio0z%2FVNqPLDCG4cfMdDBe72Cd5QU%2Fw1Kc0a3YdenJJyR%2B%2BvuKsz8owV5LatVvooQGFJyxkAlvtRmfDRArlFbKKMTtLTMLQjaIKVytHYtR5%2F%2FdCZYuGYa35sz9O2Lzp7eCfZfWmK1ffljRHXWODIeosE2dWIV%2BDfGRaJUag5UfxAzDydSmDn4rURs%2FvZlzflFndCbGp%2Bq%2BmgfhmMPr%2Bi8cGOqUBR%2FL6u7NW9cK8CDHjKec6umIGm8S3Wq8Y4sne0A780uswrYrKSfuvUHFoCGVO9DhEHbryuuvZVdaKknysQIE78qxTPA52BRoGSQdl%2FTwzMHj4BkXJb2Hk8vRWUFGfqlLEcA5cvDhzn0QjZVCaLYmv3z1HRh1dO%2F0E2Mx%2B41xzVc5bFvNhpfnsnWixXUPjt408SjMtoo%2FtzKUWqxGtPFtdfjb6jEPe&X-Amz-Signature=55af283b965e2960a1c933fb8c2712d50aef42faa0f6fbcfddfd823241b3a517&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

