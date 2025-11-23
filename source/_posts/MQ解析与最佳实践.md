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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWY3V3DD%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQCRFXds8GfZS9MmL9utClFxYWTDqJW1DoH4m0dnB3pc1AIhALVWLs5qmrfzVvGYkvXzyxgqDseXWf2raTl6kc76DZTIKv8DCDoQABoMNjM3NDIzMTgzODA1IgyNQBTg9lk9oFCmXboq3AMeAqqUlUVWbms29J3ww8OYvNsQoD%2BsDyfHNcoEY4%2BGmggGZbJWanqaBO10wmQcP5Xs0HNf4RGBlPORsmNjG4teBTwLgj6UX9uMi7sZG6WZE%2Fx7dPNN8C3QvpaIJaQeklURxtd8rC7ICitGX8O2fxzgfOFKN%2BomCLkqYU86hIwJ2OdjbkKd9FpjJnHJJ3XLJNXALTzzc%2B3O4cm2xbkZ%2BzfydPftkZTmNPpevHEA2hottQ6aRb7k4unr8YwHz2bfh%2BFfJRi3hHKApiz6%2BqG%2FvCJ3Wa%2BpJhXOJz4rlhY01e02OaUWn3Kc4B5OtNZKgm02doqc7qw4PasEJpWsfs7iWDdFtDzAROCEhyRSR0jI5BpP3hneVeMkEbE8mRG2f313Qk80SSFiehwLtmG1witgbfhvi5MTL42m%2Fko1%2FCR3zIuTClbaDMB6BuVfFsHS1jljvfTbzI5l2h%2BNtPF%2BC%2Fr53Cd3StP%2Bz2WN%2Fi2Dwc6KDQHETArnUqSgl7x5J56meGoc%2FnpZdmje5v1P98MYzn5Xl6LgfWDcIFwPWa3f%2BeBoYk82PAnPXXFovaSrQRtDI%2BUK7tPehAahSlUwB8t7oKcQPj5fXvYS7waoUOngA%2BhOuvXw080p8QqxtIugrIq96DDol4vJBjqkAWoDBB0pAN%2FdIRnCCfFJAVxoMKwwbj15plngBYhY2%2F1nwunxiCsAlq49PJH5JxihGabPhp73rBEG1bAy5qTl7P0Ut3GYXCyT4nJE1qVX3F3OA4NJpzJGtwAF6nGp1kzkVVAdJq3Yv1XDn62MholiPhV75UIrKVAt13wEpqkODtsBNRtUz0P%2FQEO7triECE9v1fI09NHnuS0cwYc8dmz7MuXrtTOy&X-Amz-Signature=e09fc405155bcef03970fa2591f7725a7be61e5b3a85261b9d90d695e75bdf46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

