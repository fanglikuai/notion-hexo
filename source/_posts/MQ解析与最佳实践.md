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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBFQGVBE%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T150102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJHMEUCIDXujdPkdnhJJfuIu9Dsw7IJz8zASSKXrsLiLm1%2FAUFVAiEAry5rfBut%2BHExez8IiK2rwJI9nZqGGaZelzFYhIWWDrUqiAQIwP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFWBVpQYnsNMfaYpRyrcA5yLoTp3ITEl09mZhdlhpfKNu02uJ7jtzBIZlB5wpl7UxB5U0Qsy7kND%2FRZiM0dp%2BnRD9p%2B6UfjkQU6ObVgNmZ%2FjEM0sxHaiIvNlKtNVsKkSfYskcKGMADuQAK6bxF0h8so0Tzx5fKdP4gLDIWT1EIWmPDdb%2Fd6i8lnoCDMSYKeBsDeEM0voEoJqZcDtVDMKocBAoBVjTWkGmjHtW9glZ2JFG2voHjRywGDjqwz0TxzZo9d6Hrf0Y6KMd4WrGvUOFr863E6NlWe4ZOiKWty5DrqJHvUJj3RNUfFWNBxEGsY17QvqNoPv%2BlVuxGukzlf6LO%2BsDlWRR2Xx%2BOHIpyoypg9AhXYeqTgvQKcqD7Tms7rMXofXYxjK1Mj5Id7wij1fKZCJyubXJFSKS7iN3ygDenoAR4GBE2hLQUD780gBZ9ChOcOqizqN2XfD%2BzE5F7G2IK5wLUQpUx6oPDvBYfWHdGq9vnezn1DLNhB8CC0Gzgfy9hwpMzF6hYjIvC5aeLsMJl4wYhK18F4xq3g5LGxM%2BaAcEmuHbR2OWXRQc5eZOb1UCvvs5R3B84p7wQM4OEJZFHz7tbUGx3wemF4pFUu5Qbk0%2FQ3wKKFuIcpOkueRWLouCDdl%2B%2BhhKF%2FXxAzYMN%2Bug8gGOqUBGgvxtUkYjLjSgE9GuVz6GGKWDuYLMTnMemCrk6bb6bFxdug%2F18%2Fyet2PoGfHmLIjCAnUfwTxTjfhq%2BfXFwIcBJZ8KJ1GLAD5ZP8AOZYQ94rmKfHmSZdzfumJ8yXyASo3Uh9ModPFwYuJDSr23gIgeHX3DsWYi4mWJDrcsrnDQAF45dw6AcjYFXbld3ogtOffXp20C2SCD3H4AVMUfMliwFzQOeCa&X-Amz-Signature=edf89c0ede2e2a2c00f3218c918ea24d845ae3d470dcdb1694cb76f67624215f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

