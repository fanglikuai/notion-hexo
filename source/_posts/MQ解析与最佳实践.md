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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRMV3DBK%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFVXkWQ4c536AA9UCnFDJL0N60kmHHYtVSJC8cGoW7yoAiEArQSSHQ%2FjWJTEUFasMroeIFarNZBXvQyS%2FMxrKLwRV%2BoqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMrSqhDcBa%2BSGRc1YCrcA3LFonk4LrWU0ZEC8kobU6AOosoqCPTzKxlxWEeHgz%2FMHjI%2FjVmoPN0eApBhff55wdMGClww9mDE6T%2BG9lxvG4J1I5qZeXPrRJnykdZFIjKkyVscnif7Szxi%2FAXtpz%2FZEngi2rm4JRPbAL%2Bl4EJzR284dfrkXvxJoh%2FRk9i3puK3M1HLtVT3i7h%2F9a8UD4r9C3By3tZfqoVXIV8Qab3RrEr89DwNKrbvexYoehcCW0Te9YACylfsxE4k9vNPQhula7ZllFGktOw1CfoZqZm9tgfR664Xs1PvDBIhy%2B81XiV1iIBSJ2YKhzqAPr7hDDySfIH9AvyBV9gha46bs%2B8K4Matr7m79ogWVbHJ6cCJYLtu43uLSCf4%2F6c%2FLBVUf0JW7GOzlm8q9q36%2BQLy24eAXkv0iY46AFp%2BKf3x%2FcSK5e4Z7bV9LA1U6gR9TVm6mMDfwGf38lBMFL3TqpRAnpT%2ByOSIfYckaxn%2FdtlwNCAt7ZEe5x71NYCun2fU7zeCRSTtOjyItimxUmxjkkmRNz8OSogWb5FKp3MHxKISMGAY6cnxAbSXAtraNCXIj2eKZFB1sUHHe5xVzkWqIqQakWoPCiFs%2FUt7xRvBdGgd%2BxYzv0f1z1zmMtyOXrwiGr4QMJzF8cgGOqUBiz1Vfl%2FsKStKwjbj9H%2B%2FV0UO4STJtpDv%2BgPHNpC2F8pZGWlE%2BlxV7YoYyCdTH8fUti21rnx2M1dMLcz8mBmxpR1CJfYgoQYO2M1fR3sYMia18WZ%2BrMBbfePtshyC92StUpNXvYJv0o2FcvamL0qCi0WiAdUPNjq22JfduGH3W14XWyCRInsdj9AzhU2h8rxgq7ZJK0q1jCxCNrrf%2BXntuEs5mkoP&X-Amz-Signature=d81684ccca6b2ad9e20e5aa82100c570dcca97e17ccd240204b44b9ae5a3a53a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

