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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5HQM2XO%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDsy7rWQ%2B8N7DlSin6AUHeWJoeqsQqYbn7qdzPpUtJd5AiBSxQ0qi7QcVR9qDLZbUxkRfKPJbp1pFJVQD%2BW9TjhY0iqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMO6fSpe3Ds0v7P7I2KtwDtAtZMl1wVOeRLiaPCTcG6VPYpskWt4OUEoY67iG%2FVclPDYtZPWZwPlH%2BChXoFt18Lz4O7mwOfGcvTKOqgM5WilyCxhr7somDoeDtskjQWdXliS4rkdkPO0g7i4hqs3T2P086FK8asmodkrzTy8fduS3Rtz7TvIyj3rlZI5X6o9xTrRUhlRwF3e5k9W6P%2BPdtZym8fp0BzkQW%2BLJfiDGMayLshzCZ8SYUBNPYh95wwZNZZqqzI2chYu5f4I%2FMgaCmEjL%2FtHwDvlnhrHqB%2Btkr3k7BdrDvglEwefkrucMoYvaB1iW2X0u3LhJL%2BTcETidDQhNhwmMkMKKUywZCSA2d5PsgKw4%2Fb2FJ5NoF7DsizUQhXQQxBZ%2BswP%2FnQsbUgJ9X0pUWoUmJOhFyytxOWHAKkquzf%2ByLNm2pfujEr7nEtVuV8yJwQdjs63jeaBo01A7n%2BGvVrxPZU1LJsKxnqR6TYVd%2FZ2%2B9qkGBfVZPGjloTJqW82e5wTSwuMsfqUp2pOZPI1%2B3v1z%2FHDKq5cu4d8Pg2XSk4tskjNPmYCUJPGyfuKeKVZKk2w9xcK7Uu6xXp1WxwjqiuaHNeEC0IpLxB3pARQShrIx1H1dDyo9a61jry5ubPTwKTBmilFLfBwowm5j3xwY6pgGv0stX26sGNByyQeFunAaVKA17MaUuMvv9RZHeEcowQ5cCXOrSZSf8azYTe83OIM2pQjEI6N19B5q%2F2p0vYB7uEFl1tFCnEHiV6Pl2nAIjuzZnOywIdZBW4ucwIuZBt7g%2F57GPSv3Jk7wtXlIj4cOELPH4kdG2qvIS6yipivomXPxzjY0A3WPF%2BxtoBI5%2FTumQODjCYYQWx0wGZ6GxZ2fMz1VtVuJ4&X-Amz-Signature=7baa8083d03dd44ea063184d4584022602cda903e9a1a8f19ec2fa35a879bb36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

