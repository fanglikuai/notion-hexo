---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665N6WAIDN%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIHHrD2%2Bu0R13SBuDbZVzkHK0k2i%2B4FQpxxzzA8K2QDGUAiEAji4wxnD4dc4ZfY1OyXgI7mRSJUKeNXtS7yPc0gdo19Iq%2FwMIDhAAGgw2Mzc0MjMxODM4MDUiDB1P1JrZ%2B7FZ68vK4ircAyRNH7yhGedpotG8hSqqJkAT%2Bm1nEDsSDUr3eVA0dw05NHNqFHC%2FV%2B2M3AmXjYwuLisoD9w4YhVW0ZE8BJXyvd%2FL3H1DMcLL3QoLfM2eto4EF3Zjdj9Sirw2WLMPQtELIl1kDBJJrufDB%2BNp3y%2FoLKfdT%2FgqYdezJ5E1iiL28HHodUWXMgo5n72iY0jYrMi4Z0tXsws1yXcacvAjsk6KfiIXRmaiDhpv2sVY4HnXuNI1S2wqRR6hOptr9YHEEFdaI4W5JHElhhqmJthaoYrwMU95IiXjeUlwtKRCFMGsYq5N8NB0Nxvw3J4uKPrd8gihE9Zg%2BkGXUnoAhNTb1MJBYML7Ii9SXtIjpke1GKwFfoOHkyxZ01TBtIZFCiigPtRfdxS5bYEen0Cfm4u2AdMMvVhhIHtxGybg2Y3nOdAsG24tcJUlSKjK1stsmczN%2FWIOKC1prSMUDG5sXZH3t85rlK4C2JxyLa9A5xXpgvvzHRb8GOJJpCl8zgiHT7zTYuQT2MV%2BXubOHyKhIyKzwsd92E9TFKD3uV8x82xPOecrsRJnlmAaXqE%2B2OvtNn67mGRZZQVHGd43TJGKJM3K256YnOfx5BaQWIW5HWAagyvysP%2BDM%2BMfjKxgvBPQNnAYMLmpycgGOqUBYQNLTo8Kgc4gWagBJuma7IjqDUZ9y%2FYSf8R1o7hFH6223VrlU80rR9ayR%2B0TwmYshYol5RYol0vzk0y%2Ftt4EWvjXapEni%2FwFkl%2FClpVvtRfsPn%2Fk%2Fz3SUrtoViGh%2F%2FO9CvCExPTCBKnMuzYwRL52WQyBwE5XMhWz8jEZgziowpwK961MdcXVh7%2F10wxNIveAzzs0vceI1a7QwUqKayNHTMlBUZNF&X-Amz-Signature=d78b676866897f886373e8863131bae9c00d2cc25d8c1ed608a1ec459065f8ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

