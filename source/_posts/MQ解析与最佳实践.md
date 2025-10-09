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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664GUBURXN%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T130043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDwaCXVzLXdlc3QtMiJIMEYCIQCFgoeUMi3G77u4oMt9q664KFC6lFQ7FtlmHU76Ty4GjQIhAOsbX5mVqUPoCdFj1rHft0AppkEs1lj0Gz3jf2Xi28NBKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw%2BAGaSHNDGFBq4xo0q3AN%2BbU9ht016lq012wKcRJ0YTRjhOdI1%2BIZn275BqJZE%2Bky6l%2BrYrs55KctXXazCxr7ncleeuucuUWws7g1oTy5Tx04sISTMJVT3R45C5awRxsXNfjLUfLg6Q%2FeyKOokU1EThoy6GpI%2BZYmY6xs%2BK8i0nCywprTjzDN2eA9pBAAeNMl4VWlGv9UIGbFhwN1TLJ%2FXqSUOLl44uMlp7Ld6tKOo3eJN8VKz%2FSGHEe5lN3CMFoN3fYDTTmwoSXhLR5lhvBUs85VqWk4l%2BkJ%2FuPPfNbqJU%2F6YBu9%2FOoJoYofhbVr1C5M1wxTi6CBpr8mj0NnkFjeeMLoAGzTKSFxxLVlpGwVyNgFaQWkQBGFOOdB4%2BNa0f4rfYcLeOfTjlQYcyNwCPEIf6%2FZBDvkJmT9mp9kw%2BK5KSVqNc8KStzDhOUxUFUJWvX%2FTlkK6LPg74rIvgHmWfnTo77bGagPhC5pBPNi9dhsrZGw73K9Kx3UIfkIlrxkTCGzzkhqeUjgY50C8sJCHeo7r0ofERr3XU6Y3oBaBvzq8qbIzGLfXsBNubC2L6krxTMcTdfsWbmhVSjIRn9sYFTwqz2R4ibn7Y2XFl7Bij28PvLA%2BNVhN4WpyIVMbubNvu8ipVgwyPE9hEPFxxDCdwZ7HBjqkAdnqGg3BRyrkrw8G0Lo0oWtLrX45QM75%2FCNw%2BEZODEDWlIVM5yWuoPnwAlzqTT2%2BbN74hWfz7W65dFd5fM2eWqw46ZVF9dT9xSrk%2FnX23PVng%2FPG%2BA3hS4kdWsFlHfTxdcrhJiQXoY5BHtS%2B9VNcbr2b3DbBrJayfv5GrmnFOlBgBaBNhN4csHRFdnMvvC93YStPAVGMtlt7z6oGOEJCTWNmoHFl&X-Amz-Signature=28fd3c41751785df0262bd78f4d3d65306f7a40ae9e3509e0758649f1d69e9f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

