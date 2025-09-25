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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WQ56GX2%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T130058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDD3xO1IPFgQNirO6B4Kgjg44G6r3IQREebP8418xBA%2FAIgJlyF5qEslEWvb2owFFHN%2BRSfFu4XDrMjKInQ1I3vKBkq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDMLq70AcOlHDRtlZcyrcA9fZVZIEnclmdRh56CppcQrb0Pv5Mao%2FVNlbLCdXSnndkHICbuUTDYx%2FRB9BB3NUNftRxw7juMJ763FIOFI4BV8WeYhmogBcDZOVgSturx6bObAywLd0i%2FDr0UA1rl%2FaAQaF%2F00EFm%2Be5pvyGsAIwZERBVgSsKgiInB2jk8CAny%2Bzhf9SMURC8uk0iXbzVB8NGEOn%2Bv8tzeXp1ZUbbdKLdZPiVBQwJopjFS2jMzWyxSIdK%2F%2Fb0YOqT8nJGx99PsSCqKFe%2FQhGy%2FSRqr4PrJ%2BwFiuUAzje6WD6WPrm7phF0fslynZOK6bmCYMZe4AZcUDxZFNtZrjOmpzjlCsHFbhzbgG1LicTWkLQLy70jH9FIyFf51pWfGXfsZ8G8SQ4p%2BOIMvakGxnvybT2hL6GFywsLU4r3VA0hr9ybvL3PeYypQ5yh7azDVQHKjQOEgWWqJJW%2BngsRM5RfpQ%2FaBBhUqOhRmMmrjGtqi1XFuJQWi1SV7qXuvoiGtkLGz7Z4%2Fqvs9%2ByUAjN1AGFDlHcOEhaeFdDHS9E523BzX%2FMGsUm66nGv3e%2B%2FJjxMKtsbSQUvbqC8Ez8bj%2Fy8qstBMgHD2kD61pQCLO%2BJNnJZS4IsbBbQwupayFblQpevnN1E1pmfx%2FMNHg1MYGOqUBmWaGEgbhbw2toxpVRH%2Fm3RPAp3gR3CEFjjWbOtVmzQsvmKXaCGg%2F8R2Cw0fixh6Go5fBGIWHkEENu44bwHh5SB07s4qp0dCyc5mvsMwhk6DC5e2eq5SbOivTk8zCyZpKiuXv7DoqN%2BJVXjvJxo7ayoOso%2BUCAavQt%2BfjBniTjWrS%2F2F05AapytOvfGd6%2BmlGLBOAsSWaAcxAje7WDCtmTeTTE762&X-Amz-Signature=b3f4a96843d100af5400f410190ffc857931fe625e190510bc5a0babc6742fc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

