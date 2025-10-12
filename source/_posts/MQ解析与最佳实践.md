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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HTN2VKR%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9nJho%2Fz2OjA4EkYe7N91XqGnPsYUyFupjuVduAfB8ugIgAw6rEq2pmvdI%2FT3Z7LqV0RjeqyjUCTaM%2Ftr80h3PsLgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKBdP1A4XUFa8QvJRircA8yVlHAYn9NDzgfakiLVOrPGp6yKR5baAL7P%2FXcihA924rQmnF7NR%2FJ%2BpEpCIiMVFAg6u8GP6ZbCW5jjscm%2Fp05cn%2Fi461AZvY1aoxDjOwGJnq3lRpA6O6diEhB4caf9aoQtODE5Vp1kepApT3gy%2BO2S8EPrTgXq6A89AstoggmNVfGuucEsnqZgJ%2FWsa9f24lp5HAoIl0aEtxKy8knuD01oQLETJmGhzbISDUTT90QnAvrsMFg%2BeRipZndMveEjDe6mjlKjP3zZ8GwqU73dISwEULGXwfagD2sbVy%2B4922J0rRWPBGpucMDqZ7On5lzGw2YLZ34Sne%2BKIOD83Npvkg7HfZOWnf7av%2BVXSs0UcLvZCYXv3aYyhW%2FmuIjY7JVRMCM67fi4XfC2%2B2TyTw3%2FRH08VLGskLNzJu9ESpU%2Fz9miK15u5fVWqOvjhKQZGRfeF18p1BC5VLqO2%2BcIcUgriwKxyVcqfPYC9zHhEnnOVX%2BMpzZgwwKB5fm6mlZ7bVefgxGnI6qW0t%2BxfEUuAcw3dvupLFmwKxdf6SaXGOhLKTTAITZeTCRnilLtx0bcYFC8DHn%2BVkjertISCrYwATZ40sBZcVO4RqCFW%2FaC41N2JiZkWUjyqHgb6Xiuex0MOvLr8cGOqUBecsLPoIoKLwNHheHtrfJ7YvqPj3Z7qqtoziWuA%2FrNn2xOZ8ENKCgt6jiEQ0Al02aCYqE80uzlyuqGJeDP%2B7nKExOYS4Oa4v5IaBRe31gO5tUpFUeCL%2BFethUsOtwACnTPspFOvnnJJS1fl%2FRYXI8%2Bey70CUpmp775eG7zp9%2FspqmcZ2hT%2FrndPUQqaqCzT1sblLRXrBpP2cCMr73QJaBZfTlP%2F4c&X-Amz-Signature=c61b6f596c1b5a46158f05a2641276584c8454e1386da447a6cd2ca326790442&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

