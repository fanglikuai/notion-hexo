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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YZ5HITMB%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCICVs5pme6x2opQ%2F6HE4OcwOVjzi9HfWgGvWAogWPRtM%2FAiBEYweW%2FbH1BE87x%2BJJRzQwkjbg%2BGk0Fu42vGRYN2SxoCr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMlp9Ofo2yxz07YmUvKtwDyPRlAKsQpBeRjTrx%2F%2F9km9n1kvVRzWMPJICYgwx%2BzWZ%2FZ13kCL4BCZs7LhEVn2AARWR%2BLTpN%2Fa%2BHrVTxVA7Ep816JdO%2FxAROxXL14RPWeNQa8R6RgjArAW48iKeJu0arjL216IAF7CNuxHl1Bsky%2B7Mmm%2BhSjNa5rw1bOuEs%2B6xYlchlhaSsl3rBLiovvQpvidySCqPIUKr6QnJrE7tw9WEj7GqzKdz7CKOwisN7dETm8bJMlnEvKK3WX4mbdvlNL%2Fwqd0%2FKmMqlDUlnabT0Nb1IvTc3cplM1%2Bda68IHtjXMxjY0AmcFgoO7fd%2Bx70BaR8ZCST3kRcYqLwA2mNERHXsfodALmGPgLobZBEylpQ7GMK9T3%2F%2FHPrGnHhY9NxxEM5zj4ZnCfL%2BwKLfhDNPaUr2duKKihODZnYu4PQgsZV3RH5VhoL6zGQcGWYQE%2BWDCn%2FW4BZqGAaPeuHd8Fss9vgby%2FO8L%2F1wjQB4p9IHGfRc2yzb1qqlMB129ySYEQdAyh8UfDks5trGNu04wAr8cIxbKRGX4LkPSJDiF7gd84uG7XZciR9IjGb09Ih9nK4btWUZT6BvQQpIGB8pTmHvEP7tll1vWBWGr7rqfuzcHn9vR66YPtfJ66O3pdywwr%2BXKyAY6pgGxraPqJiOo6XOz5Qfkif0P7oM9Pce7jB8piylLP%2BQQ6%2F6mLmeYkWEVgewKS44jLkAcIgwtNQqYLsUoG7WP5bWNU%2BNH8iG45na6OKoZMpVWO0Nv7FQCdvj2oOavfsG3QJCoBqDK1z%2BBWzBujN5exRm5oaocYmaOQvcoPO2LEphO%2FLnc5%2BVb1R7N%2FewyyEbPA0Dr5gdt3HPGP%2BKLt9agLA48BgyxtVeU&X-Amz-Signature=612fe03afc44d5af892c058b180b8b8e9944434ffb081a18e397f62b040e36fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

