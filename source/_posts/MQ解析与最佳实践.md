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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTZXPQNU%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJGMEQCID6gV2aM1XhHhl6XSZWrFfRSUnDuYZAtUZmcz1zWqYaeAiA4eJrQ97YqXvAHvyOqjdO5%2F06osWhFIhAxEA%2FdgS00eyr%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMCbpcEP7xLjhpiuSSKtwDhwW%2FK13ZTqdHNG90ZgJDFEziboMPJnYxEZD0eZhGmSPTVdZWAfa2EkLe6%2BiebDTqSQgfKW7U7Yi%2FIXSolH0lDnPYgrh39swklok1L%2FUanaVbP7RluPFQ%2FX9Ywz53EHSYYQ4M9mfZxF8MMUkpUMR8n6xdAJI%2BuKRQr5lKQsHYv4HBNYlfSak0XjHoScIGlEo2KPUttsYE9MXy9fZ10CjVUTglTr7XwWbsaywx%2Fk4iuXsT%2BfOqZ3ZhJrrPXtAacNzQ4h5S1l1ck2ievrtzjZ9D7qRsG1Tg5n5Y4kjl26TDPuhtVuEaxxiTg1DvelkpBz9j3%2FocVOIxaDY%2Fln%2BAvIBbt3THY9bvcZTMq36wD06CTddkVCm%2BQn%2BY6XcUIX1u3e5EEo5N07IkCaggb%2Bl0ywB8%2F7%2Fb8%2B2HJm1MCohvTuIEjxG2ZxOQU%2Bqfl9HCVl6icPLpXh9scowKcwHECMOi8tkrgeM7ont2BNViHGQRmStTNUGlVbKZIZsdFRE8SD4bEmu0tUcNzufHIQOok0kqy0XZ10fspgq5Gx9S8uNBFnw91rkohWxve68jgFZePlYvQHwFTYefFmwuN0n9uLB1cU%2BcXPXBlf%2F6QIjxM9YQo2E%2FViYekCoSz92edkP4t20wv%2FvLyAY6pgEvFfA9EGTeozW%2FP%2F4Ojg17tIVq1XbkSvas2bhSCrG2jCwV%2FrNxxUS0d%2FXIgdWHzHe5VoMTMA8IYTdbEEMKKTq%2BU8Mi7wzc0cLVE05rLHjTzFmnYSHxEGKK7bE7OSlRctJLz4xo3DCCx%2BkeSItF8irGV30MaPIy4TXIY7ieFFagWYQfnfepEx%2F0q1JIuF1BEkV9S%2BW5QnGqJ8HPdr2IQeEdfHvSnSKw&X-Amz-Signature=074ec564f782d5eec28073d3f75ab98d968e50b3372c38c4d96a077896a2b588&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

