---
categories: 整理输出
tags:
  - 分布式
  - mysql
sticky: ''
description: ''
permalink: ''
title: MQ解析与最佳实践
date: '2025-09-04 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663MIMEBE5%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T030043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIQDn4FRP8OrNS45ylXLTKcsr5eNFi6yHvzfWMheU89%2B1yAIgBP4IBoOpsfG7ghxXXcrKybCpbbB2O2NxlgwTUUou8lkqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAVNLZCP1wFg6VMGjyrcAzaM5HnUtGs%2BqBRTPQg1M153swGlclEZJiUm3vaDtRHl1Ki5C6aUia1kQNzqVAPSXWB9aGxcCfIE%2FvWbdjIX3buMFiObBGq2nqMIrWNHw40XJs8PwfrHHT%2FH4m43TYP2tiQ1dGEf4McMdclTektxWR0KeqAZHDlWltRae%2BYxIpQRI3HM1UgY693%2FeypOT56fq8Q6Z751rzkTf0QS88twANxBUfXtHN10QLz7LbqK3kCYmKjhXJTXnGnXPU3WmtVyRKOQcWb1wyRkVUMGPkcTFaF6rfNKSdRIaIJIaTycBgUwlSUm2GPmG%2FZwlMo48ajAI0RjMhQmbitDec94DIDq%2BeXVBtNxQD7XCNJlfG2YekxpfZmOXOSyDNqoRODTBIm7Ls1VTw3wHZZfg39REN4sVvj0zakhg2wxm0nt8JTAEsDzWMG6exc8RU4KP2%2Fd8G4Tbeii%2BfZAkYa1hBAdUIy1Z%2B6xuaSvxOkdTK1lkwjjj6Kv%2Fza30rMkANpxyccJNpzG8L8axqQy1VcET8etwuRAR%2BZ16dAV6e8ycZfVr17K3rkjoIj3cBTe9j1QTA6egxEDgXxGtEi7jNUwz3JI8q2ERtPqz2BA5bTtrQBwKAXz7lt4IUIb4uvIhqLkboHIMNKCzMcGOqUBuDtAzW0xx9MJlCsA1vJy0qCqbB1XBT10fPHAoOR50P5rilvXfNhCQGsz6hqiDvPzaphzywRxsNKS5pBSnx5RiSLrMJcPXuvTOeEY9LQ4sYv1H48j7ui%2FKqqZYNBBk1B%2F%2BidJF1Ezia4GsNqVxQxuq6p3oq7bBLZiV1aeQPBQiBGEA33rtVMTrijO2hbJW589y%2Fu%2BjSn8RE2SNT9EfzkvjOwS5oFK&X-Amz-Signature=84b4e9d553561a782ae659f451cbdd27858a3f133c2ebe994ee9e92c99a84d8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c8962001455e38177108499d1e1e605b.jpg
banner_img: /images/c8962001455e38177108499d1e1e605b.jpg
---

# 丢失消息


## 生产者丢失消息


使用ack机制


异步监听：

1. 成功/未成功发送到交换机可以触发一个confirm-type监听
2. 交换机发送到队列会有一个publish-returns监听

**但是一般不这么勇，成本太高，丢失概率低。一般都是采用日志/邮件记录，手动维护。**


如果使用定时任务那些，成本太高


## 消费者


手动ack


应对：

1. 消费者失败后，将这个消息存到redis，记录消费次数，失败三次就直接丢弃消息，记录到日志数据库中
2. 直接false，记录日志，发送邮件等待开发手动处理
3. 不启动ack，使用springboot自带的消息重试机制

# 幂等性问题


原因：生产者没有收到mq发来的确认，后面本地定时任务把错误日志中的消息又重新投递了一遍


解决：


redis中增加唯一id


# 顺序性问题


使用一定的策略，如取hash值等

