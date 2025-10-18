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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQ7TZF2D%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIGaZ4eMYpEuA2tYT5vE6%2FqvePqIJwYMq9NHTTWz%2BP7sqAiEA3RVXVgAFejrmJ4SARc85nHGbd%2Fkdm%2BIs%2FyOOL3zN6uQqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB9zKrmTM31xmW6oMCrcA0SEW6ckyo3luzHZOmylXSW5QT%2B1F6K0RHMVrXEz314ISBtawNtuF6auipnvUnODfTmIKtg4MwNC6jo3E99iSyLR88zZVdhpt86BiEnAlV4rdXgRniCHqnE8bmX1JlL53RA49i1QhOqiXe5TmxU%2BBShNzGfdhoAlJnDVjBOnI5f09FmadUq50YYzn%2Buuo5kFV1t0Ea1FMHgpDK%2BRsj%2Fv1UXXR8CejTMGrsXF2ab%2B11lOjoL5fLwjuK9AQo0VKPwbhW0LJNpN3sub27hWsAGS7RYuCDTg7c%2FxY3rURGEZFOceitJWROH4LHLRFGnMc%2BzSGtY%2F9Ka6iRCC%2FNYmV4pr2qOGIGpal31XpcFVkrqty0OaCCdvUhwhD1XePGAAhzq0s2H1zT3h%2Fgp%2BXCEg4iaYWhuqi%2B%2FUz7I8o%2BM3Qr4Mk4o0XWcTwyYveIiHctRAV0L9CN2y1Ok0xVD4DHj3WSz52KF0Lj4ouj%2BI9vYnS8FLEFBZcxD22%2FYxv4c1wZIrm8j51HB%2FuYvPov8HlGqd5n8KTNXLpBdihWGmD2XrIwz%2Fy2Reu3ifBgR2WTcjicB4yYSluP3gvkzWJY%2Bd03liqqgIDdBeJEv2vcqCPwdzkAIWDOwXwcph2vWuoMRNSOYnMJvIz8cGOqUBar6grWsH8Dl3deEIV7qIkSmlznWLkbk7EZ9m1m65l9QsQ7a5N2HKrMaFzj6jDf1OpNdCA69yvpV0fmrmHqBctKG1JHrf7F5w1YmyyKjxPYxXXyqmZXN6dsoFeFS%2FZ83nFL3HigNomEeNjJHRv2TqKt42md4B9%2BXAaZkzvZX4fbMSreL7tQP2TiN0s4Uw8jFYZiUNTdR%2BaD1BDX%2FU4YwUcupL6wTi&X-Amz-Signature=c04b7e3dd07011d42bd598fd19cfde605eeac0ab3c9f2aad594d8a1f1fe7a26a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

