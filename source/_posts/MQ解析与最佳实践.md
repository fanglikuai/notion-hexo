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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKX3CXW6%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCG6ul5T12a14Th0wSLyT0Xk3up0YU%2FKTzHLtd9gRp%2BDQIgOvF1A66UgqY%2F1H7R0s9J4hs75Eb84khDDK8U%2FEZR7W4qiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE%2Fn4plqcrs1AgqwQCrcA44Ukzrzscb7UWtjRF13RmoYL%2Fw1KQL8%2B6Cb7ztHK4A1nvo%2B15CGs91XYgs%2F2ZKtxX6gw705q%2BzBfjEAe0wsB2lCB9q8r5DMCYRq2Z%2BFLtnlcEogX67w0Kh45mP4RKHPvVC2VjYMzVGMJcj9JWuedshnxHcNXuGgErcaCZVtD7%2F1%2Fs2Rcc3iktG%2Bli7Z65xfhugwMhFACSCwLBlvJ5klcXU6ZmAmcYw3CSWBZQzaRMlyUYKy%2BGsGbLB6hDS8W%2FHYjC4Eaq9FrhZv%2BWrEyaA6QPW27EvK7WrWiVKrkxrrZe4fuJy9bVNgkBUM%2BclkvA19ditYx5S0X8cwFq6hnh5WOhrdz9aVvYi6S3y0b%2BuvFooH2hFY4jW%2BpyYUnHDPWAZ%2FUBGS1JzjW3jz5R3Kvc6%2BeeAqjkHihi32i2VJnH106HOEAVPXPUdBjlacvErlu2YHv83LDmLlZabLwwtwIA5aQvUrsrrax4OdxJEDByePO1KKziZ2OxTSm0EpSPqfoULRQDudpfpiVUGvehYDYrtLz2tCzrtYCPynT%2B09FZxGcInHOnv%2FPn4J%2B8Oc3OTIlOw8jkdHObXNeRjkUWT3oArCITZW1I847%2BTzVW3akIVg0cwVosvdtyeHhdGhUO4UMN2a4sYGOqUB1eITAJrYW%2Frez8vMCwwlu5M76Pw30A%2BSucIbS5Yfv9d6zlOCSj8U%2BNqsIOFe00Yu5%2BwP5dXP%2B9%2BlfaHgzGRWOHSnFlVwHOQKjYhRkWki%2BytkThfyb40EmJe0v%2FYIMfC0O8w%2BW2YAAelWYDpgYUad8OWRvAA%2Fdf1afxlOdzgUF70l2cGY1JCC3OD3aTKFhGnYFjw2gGKIRW6RKwO1QoygAX9N451X&X-Amz-Signature=907cefbe07f273896074421a85f43b61a197cf189568ea64cccd3c9e5c08daae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

