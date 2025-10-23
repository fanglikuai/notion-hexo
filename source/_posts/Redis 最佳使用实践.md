---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46672DOM3S2%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T030057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDe9uZR62RX%2BYz3pS3kzbTwCH9gP07V7MZeDWrxyEM1DgIgYMNvecYNC%2F2J64BawkAuHfhi4k1zzznks458dW7Ehdwq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDAzaFTkDV35M4jCLMircA9Wkdk%2BWAeIxpKNQHyeo7HG945mHlGI2NAbh9z8Gy5uGldiFTbB7Db71mXfC2xCZFQzf3%2BfcJpEBH6ReEJR4ed15DQP%2BYVlbLwzr5qfpsFiwCchLf5DBIlQQzuL8iiLntgM2Q6yvspR4ctSJGGvsEGwyClkXriCMKTDXSPLYLBchvbpWtEYrqHzhNknz5vyhjnaHrA%2FL3ZJw5TjYsBFmbE%2BwoRCeXNjAOUSFysjBLZ34At4I5ly%2B9wc4k6tfcx9y%2Fwo0DsHhkwpp%2FvLWSMOTJNJXfK1Cp%2B3Rk8bd7Yh%2Fc54wfjZjDk3GfoiALrpgsqsPm1jAuH321gC7WOju8COFiZCEWkSSsv6wNQR%2Fkju8XEYJRLWe4HlTMfi2Ic2X5TdZYwEXlDlteidR4qI8CHArP4WQ61K4nGBFxD4nmfvFeXKVbDu%2BMm9ss4DI%2BcApRin6BgyF1BWTq1TNWaXw321n0jagMyiNFroKT1ih7XBufciKVxXvqzlRAcEAasclwGZvCUypbvfKMUCvCda5Nnrh2Xcp57i78oOd1cYzGPEyHaOUbuOITgaBfnnbMwuvJ2ZB0X%2BQY%2FkdirankmOAPNDZZc5uR%2FjUdyC%2Bo59WW%2FBZ3nuYLcob5o6r9NLx5TpOMPyd5scGOqUBARX4KiLLFuEiiHOjX9D0KGSAVt4no9LzmjZE%2BPUjI5RzFjKUT%2F6670ql1ucCQh58P3z4%2FiWxX%2BdbYV8xEBOHwjzVRY8Gi6hl517EiTHhCmXHTIZOBAKqu3wGXnA%2FlgVomh3%2BCHTcBfNU5dRzNXjpeoJFUJtgdLs2VFn5h6qhfeBxE1QYlKWuNUVleWDpkiELBEVTkocdFCQvNRPhDBWPR6hAGT0D&X-Amz-Signature=8bbad6cb3584ae890fcfdca2f1026949d3e8f4c47877f85f3e600b438c0d67ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

