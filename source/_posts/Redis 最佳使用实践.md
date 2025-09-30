---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TLT24GYK%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG4aCXVzLXdlc3QtMiJHMEUCIQCXTW7OrAWsMwGPe8lfJVo6Dy%2Fsk6nJeRURFiLXEEokUgIgVVTSgFlILecSWgmsfvndw6OVRb3zFteo9yMYzfvWZh8qiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNvAgAeMEO1cp9ryJircA%2BfA1w7AJcxYjFZqvlNNW%2FBs6F8kvbZEhKudjq22km9AVjd%2BZo1r9mJOYqlLD4zoKhjpPqZ6uy82C%2FuY9p8iET9TsVJYV54uNyDpxBuhTHCkhDhPDQn%2FGeXn%2FBJFb8P0ZGdk8UjMD14VZDh8eRnz3QBOsFx1ziRajFUMbylchA6BkVrj2gJZ6Hi2L2OdzaSdahOcmn43koqYYo83QqKdMe4bqQ2KeQMbQZ7mj1i6x%2BKJ2FXfAG9GPTfLN0qh%2F3skdiCHIGAYG3aEFyEPCDnQ%2FxtoFlBJw5Uy6Ub3FEKsKgIhquUqyc%2BxIfDObmJ5%2Bvm5SPHeO%2FLRD%2FhmMbQBLQakH18US9Z1abReFQRjyTM%2BtcfeQktVhL79skr2b85ZF05%2FvLu4Oogqrs4U2CkHaLaxPprt1mp4x2L%2FPPOSwXmWWNY69M%2BbM2xZQp2g2eQ9UUfF77JHzpnVe9bZtUkdaf%2B5LwjHARbF%2FfG8VgDLTo09XnPbWXTP%2F5WqaJGEEoijMh2aXOXMGEqJwfIfM24FANC8Dt42MXMuonYE%2BbT0V30XnvyB6J6JCDEg2L75fJCKRoKgjjekwpM7CGeEzlGCO1kfWCABAAz3LQ4YeCaTyrfg3VXMWT6ePTBs1EB2LjofMLuf8cYGOqUBzVCjlaL2I20giFaHrwsQP4t6MkjXdtOGMz8XfU8iwS%2Flxa8qJPSrMvAMMRfxHZutqfXd3OZRo5ztgCogiZvUmChkd0tsd6iCdkHAF5a7XNOOcUviGfEoHc8gJ8w5WEAMebTJ%2BpsSWDtb2rNiz2zJzbSsveLU7LSXBAAIg9Lenb5t6stmqvVxlBAn6qdW7gYfh60DaWWYd%2Bx%2FgopLeCHvw8ciJxmU&X-Amz-Signature=d17402ac6f1cf4fe4e71177916064a2897e4cf20ca760fcd4ae4799de208edae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

