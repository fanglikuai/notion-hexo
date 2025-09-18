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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQJENITT%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T110037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDEWsSGAwt0VhuI6RwzGzRRvWJwmPxJCzNsz5opdn3lTwIhAP0yHzWQbFhqTtEtvyMKXLi2g0wrHwmCg2Z0IKVzOTqPKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzZsUensOEp%2B9BCZL0q3AP%2F91abowWy88GGQNKXI3T7ywHeIjCfEYIHYeIH92B7ynXzC4aILntZWvyQNc9K4G7q6XcWy1cfDE43MXclxIrF6ja0rkUOtim5esqzVjaYyvT5YL6tNKXS%2B8tjiewyfP8F9iKgsjjSZ7mAfWNjmzZQRMZWG2FlD3YtiiWe4O7%2F1mUUC1kDj%2FoR1mcqiWzUXEmaMCGWHR79N0pEA12Emp0Pmkd3q4qeNTcyaQ7OVjVinVq2e4Brsaef7GOYXI2tkPqxIOyKvFnVPf45r%2FDETon9QXAZi8z6OlomF2BHRwOHyzQDs1avxJsg74BdrC4AFoz5nQwyuHql1k0mkHUYqEwQJeUoxEQlhmHdhKZzY3y7ABbl1PGKDj6NR40aoVg4pH0w6o4nEIf6M2R%2FSBCY8zj%2FMsIA0UqD2zOg91CwDUgqUcGG0BqsrdyBD5q9gnlTVUX4Qr7X0fRdqtEooygvJgXvQpT0aZf8jeSsR6MYkOKFwun6Y176kFk3yX%2FF58X0XckFKLus6e1AUdt0U%2BTShuYEKhtjI2JFTB%2B00nZWdz2%2BpJhDGL8gPu8YTK7uVsxFWmzjGEgYGHxPFdzGIAKMFsGhU5tjeniewOOsv0fkKydqNfEDWK0sexTIvFNRMzCgua7GBjqkAbeq0ErZyU32Pqo50uvoAWdXKE9t0XyJTO4Pezyr7frfgLGEYlUfITbb%2FOujgWZ2W7kA3zMGGkW1Yd5Yo8%2Fjx66jXJHUZWyV%2FWkiEsrDy%2BQdWXWGirlGFZFNS48JXKD0mydy1iMRtjG%2FsEtLi1GCFt%2BUXEcdPHvmoeH4hf6ZSMXhh9bg7wkqW4ZD53NdOMQEEDWmkezyC3ine0N7Ii8cj3N%2FYIr%2F&X-Amz-Signature=a9707f72d2051e4c9d929fae41319d12a65222682e38d8b64a6cd830a07774c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

