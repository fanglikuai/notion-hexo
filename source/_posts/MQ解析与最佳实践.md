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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UNWZR2IV%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T100649Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIAlyV8WVAAgTreZciQnoRzZPBT%2Fjz5fsqd4ufmt2Yjn7AiEAs1LavMa8IowoxrXNbk914z5roVR3roHO4uA55kHfa7kqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDETky22Q6Ro45I3%2B0SrcA57AR9Jw9C2rKjeq52nMlchSSkWezIRERT8xnSV9moTwcuPgQ2oOUXTJdwjCb%2FOV0Mwc5wp0z21aHpRREGhpTPBMWcjbbhfpVDm3Mglel0ZIAgP5Cl57mmywVjz0axqkf35cvi%2BjgUxE873uULd9ebzgLtRleaW2p2cJ%2FoP5WCvCPTBz0tbzZ1GrPkEBFdNA0CJc01yQErev2s44dbid0v%2B4%2B2EZ4HK%2F5LcA4RYFfFJml3k9xExXC%2FOo%2FVCmSt4pkUwCRmrA%2Fr07j6OOnnd8QWiVX7O0qVHM4LjhRyKC7GHY57rxnyYgR8Ke2mOTHkGmeGqX6eBx39R5Anzyl7aMMql48mQKO2QKkUuoLgQi50o0yTK9FQfKa0ssh7OVed5s5L%2F03Tim6sFV9FWvQXEJY34UgcoLLvBitNys0NeJTYIjLxO3eMjAzQzUeB3dJRcn72YF4%2Bhi5X3eq3OvC5tuw%2Ba6%2B2rXroCtQ09fCF2WL8QlQS8cHsfJQHm9OkjDBhVU8Bo4eUAJvO%2BNWaBoKXR5w7d%2Be0wS3JpzBByR5dhEd%2F7bKNh2XiuTczBrOHzTlCjgWno8UPZZBf9aS97CN%2Fkbtcge52vxXRt%2FZS5vL2KWIfwCvNQB14%2B%2FF6zct7ciMPa218cGOqUBBUNT3%2Bo57yrEN%2Fzi7TqKPxPnMA8UttSzzYy6ao3i%2BAYAA2Hqpd0sruQc3a8iZoZGjM31WkhGbi8cfa82d3WtPTlACTv6wWiW3vP4lSM9KEeIxL6ZjtlGolxBLzji70jeMylnL7bniE6QNlibj8949bbyuyX%2FbQC50IuB7Z8xvydeuhFyEGIIWimszKPdL26GDR53PERdsFnoluKct8Z5YuCxjdH0&X-Amz-Signature=010b4c8af5fa13fe254e2a6c80188f736c4bb361fbc96f39f175563fe1dc7208&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

