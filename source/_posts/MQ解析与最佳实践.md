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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LDOW2R4%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDOFc95UxvBPloGCIqNeevGSGdbM7EShG%2F8r2j7mcbvPAIgFKrk67QqG%2Fn1DnHeLpe8lydrCZtxiSv6W8MsTxsmXvgqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDt6mMbmIvqE5QcEcCrcA0DcPDGfsl%2FO09D2XEk3dwtr6Wog4xpeFYgHtPzcrBexlaLE1ZeE%2BD8AuV%2FsA6ZfNDESnoBuRMeQNj9NI7dP8sasGBq%2FAcZ5iZZx3dxo8rRwzwJwSti99C3Va%2F3A7SD9B1k%2B73U6ZIF02vEvveq6768oGx6qZ7JUPioT%2F5QaBtw0N1YK5REtqJ0IHZ63CQ7iDlTIL2mhOTMkpp2PjhdwGEycF3XfOv1Qp%2BPtHsRPqD6Lzf%2B0hkg8NtpA0YuMKeeUQbtOZbMb6J2uyclAIo%2Fdl1kwrBIfY6e5KT01CjhaP4JfLRgFo6WrvP8%2BoQoUt1aMjXw5qe5fNEJkvdhjK4O0TNpc5RbENqRN7bvmrMuuTRrQUeIPkG48yiasyFVsX%2FVmcpEvjtcJaupEoycnkiYshkjqTEGvs6G%2BMFZMn3dxsVJ35YGS5gqu49e8YYwD4A6klwYeQ2jrG%2FThVZtr1Hm7sn5IByznVnNEKM8bbH27fMDfWEaxO8jgydrVgxoyfewcg8IWYU81xoMMSpVmMxsO6sxNSncJtqSuqGuLlHWoM1IS5nKPh2JY6u3PCf4gU6fltiSvOfYWjs29%2FD8SzJO8go4VWVXTgoEWWQNiUwQ942nuBKLpw4E1jgd845a0MPzCxscGOqUBUYoaXJGTr1fmNJaF8%2BPBqnVfYrN1EawFu71s3vT0YIKfBUg9%2BUykGN6MukezO0ApqzDqgN4LC5rf9BbuK7SgQ56e30y%2Ff366SOMRDXUlaIGy3OMKgABRPAY7BVIP%2BMYrVTBsHRobchSzCKqlpsLVQX9Slzk3NkOM0ekDbew9XZTevJMAnXeVnly5c3SsHeFVhFkSPHVNNdzpI0J6jEaL3n8YcmDg&X-Amz-Signature=e7c7e832e1d6df442a23a71c8d543a338731a9a333622f8a3b059e0bb82d4c61&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

