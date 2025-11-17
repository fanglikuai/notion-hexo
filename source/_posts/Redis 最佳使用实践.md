---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QG76GBAX%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAU1uZ0IaJEf2MfgKuSfYPIbr%2FE5%2BU2gb7GqHJtzIjAAAiEA6j%2B6IwQntTGjrOpycOB%2F8UEIN%2BUlpBPtUV8PrRiZB20qiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMjmeAPw3MNm9m82circA9WESGs7i0AdEFdPB7nOVTP8CK%2FYhVxU9Cy1EpW9b3%2BAh4AmQKDmYFGoOSADt9u%2BgDmF9ib94%2BYJvhljoSC9GA%2F7ZrvA0KPFm8%2BXQbYWVXaws1B1RfAn0GuRBiO9jh%2BAU0UBuRF0MOu2EwKxZzZpKPnlJbw7LBMNpm0pjtCnkNKJMSwIAoG42YHF59RM9acn48UBBAduhBpT7INmCxGlQEWZ9TNLtlupBiaZSjM21LnQKIDFu6i%2B4xQLK41RaUt7AIW4pQZYyQsnJXlMudfc8hTP2aWu2LGOcAp0uE5oQg4rxfdxxqJxxOZ7IFg4yDAe%2FweoIRPgQvCRIvZpTsEZqpnvdp4KqxFD4X%2FELfFS8DthxhZLn9ywGGjezggLb6UAHHBxnQQyMYb0QTTCDeRJCCzyypK9R5YC3rO53blIa9DCGmdBk77aVy86O7bex3CTI69Ib1xHCthgeJ8%2FvM1pOFtkj3Z6mSlnVDKtl7yHVM2dyURTZRfU2Kih80JT8cCLY9adtIcujXIY9fMRKd8vEgJhDDb9bKOzNdlalw1ZAybORGZ3%2BPxuZgTi4ZbiimGJMoHzxaQrxa3UKw8oL7VK8aZYVvkMS0kbX4HT7j0hMnuL%2BKzfCQPaToMvnp62MKb%2B7cgGOqUBGZNLcPZjl%2B8hviQCQXksfu7f1nHdt8EM%2B2AjC%2BcxdJ7X7RwyvJLbnAYjRAsnWFJiak88gZw5%2F9XXRjOJdY9ojFy%2FqNvRXnv6zAbwbWCP85ohywRIoMxQ%2BNn0Xk9Cuw2HWbbBGwc7TFWaFUqZ%2FxfgusbRc9rTca9Y6%2BgqSDUmKNumZsLXI%2BcOJNrk6zqkNeOYMy0oNoUOWGYqSd5m2OCXw5o3%2FAUS&X-Amz-Signature=afc3573562598fc2f5c0b15251d6a118a259829d03a62ddb587841a1f6bd57e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

