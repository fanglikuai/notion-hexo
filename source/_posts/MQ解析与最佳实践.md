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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPNHRJLS%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJHMEUCIAq17mbXBT2%2FMh4FXf%2Bd480WBjuJoVjb0RB4bvTaAU37AiEAnxNlqfc0Udu8G4CmaWPVv8imgsKpc7vT4Rt%2BEb3SeUsqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAHk0%2FZWvGf%2BfVlyhSrcAwEpJZiZOW3yOYL3Qc3QhUHBjuqtmwJ6OeeJ%2BQV%2FQD6l0l4CDA4DW6l4OnmSJ3kg2Ri%2BTzKWgjP2otBvUI39qYlgfuqud2%2F3yqobMY1NPm4PolKhVRWp%2Fs9xNs%2FAIIukyg9jxvWaMr%2BMd%2F3OWPusdN1EQ9uA0LQ4GrnX9S80qpUC81GsEWCs2sI3m0mkOnrdMQaZFgTkv4m3GvuOWIiLr7x1wJscKHODKWAJXC0%2BbtMsrsCxDqQIcwQ3SMZl31CTr7LQYxl5JXvoX8gpbGEENdaJSv89arP43gIJyArbxdybDeutYeAMq8kAaX45uOAzLUeoXnlYrGQ4zF9FXTF5fyGHzQW%2BH0GAnQDeIwEMVb%2FZYgvlDNi6XF4KO8TagEoi1mew7qOlagm%2FL90IL0Mtz%2B%2BjX%2FIDGluZFARRGQSgZypz%2BpWlWIlt28BZGPyAH5cetxgQWBpU%2B%2FJP2ugWlw7LgQpAWXbFp5Ub9xvynupUapv9xGtTIG1yfQYr776K6IEV8NkXBG2z%2F8RjBSzqRuWF6iDpZs8a0s259Rd8Y%2B9fdbQgkD0uyQQHlxLyLVFTY5PIo6nVTmNB8b%2FU8mYkKn75UTvBEG9Xmh3BSoqXUoRM3fnSIpCd%2BHBeWy4wYJluMI%2B948YGOqUBTxlQtdoTv9yESLejWTgU39PxiJNtpVze2EFT5Yr6iRDhxwGCpe%2BPzI0ZA%2BRNC7noQ8san3rrW0lX3TtmQukt8vOT3fE3j8QGHBbmZMqmzxWiC%2Fx4a5vcjnwDaI7GjFeYQm%2Fh6C6B8%2BGDB9WMb9jdBq3AXJzt3i44Uuil09jUAqclfww5HNVwa6AnfYRl2I9hsTJCrS0cust1V8yGLjYabNn6uZGO&X-Amz-Signature=55e493b9a82574512218b9de315f49a88d0f294c4d319d719e02d61e125228e5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

