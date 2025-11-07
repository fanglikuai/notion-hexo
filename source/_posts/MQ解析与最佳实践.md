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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QY6SXT33%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7yzze6knNx9BfcEwBXbPgq%2BhAbGkgj4knixG1%2BnxyHQIhAKQE5s5ccohyAap0CnlDe6Z3d3%2BtxBCmwYUQ15aZ%2B%2ByUKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igww5J2eyFxDTU7vPOcq3APltRwR0C7%2BFeqEjjDol1u7T0jA2LSO%2FOqIhuKv5ubBhJ5nk6yNJt07Ajd84XjuS8rBqWeC9JroSU4%2FgBXBkKqXc665RFjGFBn9Uo%2Bf4ajNDcyvImUPKHfx0iBfe7OxHALp2%2FOKDN%2B3JUHd5eJA94O2LGmYHQfSH596474iZME1AW4ZT9O6Q1AKwtziVBZm2nA%2BK4GZjPl3dZl8SYS9rWyI3n7TAsSBzB86B3jAn0BYP9pg6kDI5f1iWBQ02XPnzkptgHx0BwfHRk2pXJm8hENBZ%2BdsEUNv6gsG89wRml1l8ssP2dojkK97%2FYxQDQ5%2B8qeroX%2FF%2BY9RoWJmrswPyMnU88nReSuPVqHsCjawa5vA8%2BYbVmrhL9B2zTCbgZbNC8jXztNmaDvrnLC6%2ByTydQ320JhU7abDBqzNQvRS7NVcHo%2F2WJZYrxYUFZcmy4U4EMYpiow7c6Yf3DJ9PEHz1EgRKrPXcPlicz8UEfunGc0aNy9nqUhpYPoqnDA0mKew0iGSQAwxdKSD%2FiluBEpU1tG6eXFqkzxWNcB%2FZ0nRk9LWnUmPQSmxeKvd2hVARZPz25OHJSXD%2FbrIe0zenUsMu7AwJksr8OOnIqvSGUmhwXdoh9KHcNh%2B4p38EcCZ7TC%2F67bIBjqkAVH77C%2FAP91cuEHZVyEVOGNRThMbVHm941jJy9VOXdBwW1c34YdtF%2B%2BrvqFPWAbBzqBYfWDh6EWr60RfhBAi9JRCDpp7U%2FQ1C%2BI0QENjtm2DIG%2FkAG0r5Tetbmi07TK1PM1WSP6UKirvMExG0hZAUxiM54sANZtgpyqim7GVS%2FCYcH1WA4OUVaC8vfKPpAooSsgQPhlOLgfI8wn4tUsdKLIdcuaG&X-Amz-Signature=b703e9036d56e69e3cb8feee7557f97ea3f2d72807f39ca1d5f151a6214a9799&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

