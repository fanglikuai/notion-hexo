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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673NX6475%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T160059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFrOp1Mf78ctew1oIpSkWJwJiamwt8Hka%2B7X51QmY2kqAiAY9IucjdXF0EY5Ozpa%2BVxdBEXBCnxEZtch3QEn%2F3g9TCr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMUp%2Fni9%2F%2B7UEq0DEKKtwDWWi0Y2KkxGbUHCvTN%2Bi4eUkfRnUWIrC%2F3OrXwVojlhcVQ7F5B0I7UKANQia3AVH6cYzkqIvoBFjmav7YK9AO9DYilbsvVqWoO41Qy5W0wKgWA9Tmaa3mM7RRZ%2BRXWg2%2B6x8mIH9a%2Fy%2BMe6lHBEv8FMgK4IL%2BZHBs2ecW5pC03p0%2F6OXQygEAFUoIcyLkQT7ySaVOLx%2Fd1JrBuM7qyCJ2lfDYgSZdXfCL%2FGFuqwH5X4nx6qZ64ANEk8V06eOb687m5bgvSoxi5vDKdQCemSzF7fszfqGkc%2BLTDDtVrnasK8QelFqoM%2BQp6jccItIc%2Beg9uPRxPsLF2tz8IWulVJCfovMj6Lar6TuYn6TaTFr%2F39M3zbci1XCKOUrqcBJZeMFoKZ4RU3%2F3pK2EV%2FIaiLAcCifdYkRtGFevmmyGdB%2Buhsz2zSJtDW46PJaeQSWy1zcyEBmhidc3bjFhB2vJhp8qk4T8mDqqHdfCpWac%2BurPPT5ea9EuPZxaHa1He5IDnMAl3s76ggYMqyPRb0M%2Ftrw6kOPY2p2orFtqmVdRjcDnGyVYfLhaxpKrrNsHNnTk8dbyELnYByLUCpQjpl2l6NquLN9eA9F2wjgEIcQjP8UZrqLYdSmkixDKHvUudfEw7pzpxwY6pgH2JJ6iWR6f0OkGqmGmhRElZkAkpRSgydge1G4DQNQk9Ph7DjjibKRIw4006rlGADbXoGsVRzZw69AaewkiGBy%2BEaDZScj6XdfMFFSMmwUdxn%2FlTxdTTO4nMFKB7qIU3oq%2F5VaEM8mQbSw2dluJ25ccUdAoS%2FNDMURkwvfAuB1caF0aNdH3hJGhfuwd9T6mM5DN4IpAGYcR98xO3S2uZ%2BotF6oYkdi0&X-Amz-Signature=9bb4d6b492f3b9e78241da01ff1be31e829ba3bac6188f9627a599368aa716ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

