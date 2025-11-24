---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665Q5HKTAX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T170044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCpPBmyJKvH%2FSjmTiiaXmY6Ql5ShJgKRHNf%2FCMPQtTmmwIgXhUN%2B8cG%2BkKe4WirOAPLBfE0FijupOCqfWMqd1pzr78q%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDPerhspGwOlkgIP80SrcA9THSYyUbr3ygsvnkoffblp3AzXS86vBiBXdDamTU0nIsbAC2EEglJlfjD6kAWjhW49HeZ7Wd2D2iroN1iUbGlks4awfDCoFDX4ckjHmnHSWyBG2TkynsnLMqLGodYoMvHYSWYFgyZ%2FHXEiPCxuqEgA62xBhZQ%2Frq8DWxHKM2Ml%2FhjNRzwmpkDDk9trKUu4NryUz%2F3Tf%2Bv4WvF5YOGVhOc6n6RMgoW%2FAUA5cmuwj22m2XHdyIH5fPPHAuz17dgooT44h1Xc8Vvy%2BkDxxOJqBpC99ZUGk6BjdXpiPE%2FB8PmVrZ6uw2qbIRQbVKtLYuXHLcYSKLgZdq8%2FAMkH5%2Bv2EvcEabv3uFU485IRdCLHTvsoF%2BNGvIgeIJ%2B7IOs%2BqbPD93vfW0LX2IxXz0uKwvzr1nhBTFtpPy5an8J4OYP2irnXIGEqkWTuO3HnIYY5JGgA9nIlcK6NVUQSUHLjmSaKwCLIup3alUsetj77nJT1r3u8nIgFHu48OCUaCsav3DrR5PVVZDgN%2FPmiwkYlCpwTVdNOIt%2FsYwXykegQI%2BJroS7aIQBTXujXsMg8%2B3HCsDGJvSmRDao4%2Bqs6Gd30ARy5EC7DAdpc2O2jmLORdwFo0zqFiTAq1wqfZZCr4EyPLMISZkskGOqUBHZBiBM48qEczWN%2Fol47EmpJiSD0KvzEd53sVySOKJM51CHkegT%2BfV24pgAFrYQhiwwrZ27gumeV9KpNvFn%2Bg2J%2BsA34aBx9q0JFZNsxGifCTZTWs4PK0pUxxcW8rmvlqsgzXEQFPjlfYaZMN%2FetPcSyfCDlBRY03E1JevVCbTPAhaMFIpwzsFwuFHkuRCOn%2B41LBTeGA9xxn%2FHcpQh6qSLJZM90r&X-Amz-Signature=01a01243a0463adfcc3ba232035326f319ed396d746009e59872acb36cb45c20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

