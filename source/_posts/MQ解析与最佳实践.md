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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SO764X5E%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T220036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJIMEYCIQDd846ZVj%2B8ZjwEp0MIymXJ5Adn2PLmaFNX6eA7AcuB7AIhAJ6T0zmQtJYSpCb8apgn5Eztcd72Ca2tGiyN40GoqgKHKv8DCC4QABoMNjM3NDIzMTgzODA1Igz42e2YeLPHWTWuugIq3ANASOxOgnteyO4IpgI0ImAmlcv2Sd92LK3bnZm%2FfDRzpbVwzEdqyG9z19ERoYJoyVbRkqsRqQITV%2F445ojtdm4KuCgTJQMJ57%2FKGM0eZLbGBUm33DotZrir820Ca3AuFXcFOSwN0YQ9Ygdhh1JAUonJcrK0xC6DoALn1ZV9UKyShbq2ClNjCpQchQkAp8KTKoXWC3e3nTAOhXqlU60xckS6raFPgIFX4XWlYYvS3GD%2BTWPLEQeKPoum0nzCexBJSUeUa0%2BBtMHaHpNHMcCNsdm%2FmljVZqBENFE1euZWReK0VkNwoNKE%2BiMH4TTyj3z6AL74bfcRH3ZyVwovsl6Y%2BukX%2BhuXSXPzxOPLWvnTI2w0cFvKtbf3c0818roMTnhLWQ5LLwyI4O3%2FMl%2FPfWeD4P8TyUS4E4W%2FCwMNSNN1HDD9NDx9yBh9zcDiwFd0ncmIzVQoGihl9vxYYZs35HisM6ozpRqiuSw0uWd9wQCRaWCJGhExo3yyX9r%2FK%2BV%2F1OK5B3MDvPmcZassgsACyopLu1DioxWKc5lsMcoVMQQGXqOaNWIBYaeyKCtlAEhVRXYg2S3mng94F4VNjsUaQ%2Fim6cMWz%2BSpvwilJLOmRt6DKgLkX31YHBLdUZOGx5qBhjCUxojJBjqkAcG8coDXYO5Zr1E2XKk2bKnyjbNYusN1MdQEulHDPlPkwqJNN40hdaqXqag6PT%2FI6kyLlTkK0PgHBrpiW%2BPHxve1UASqchsbdNN5q2QeSRyA1vcwj2yOo00nBJbmlh%2FMo6PDtFdozblYDHoeHn%2B2c%2BQ%2BT9sYb9ph73UUhy%2FZipm4MbEL99mCYa5LBcTX2nf2MZdnfXmWWTkLwJzCKfLEtPYTNie7&X-Amz-Signature=5f6a94cac76c1c1296fe83b2d0406b9c5190ef60da39559fffbcd8a3d7a931ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

