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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NSBJOQI%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJGMEQCIGK2Z1mMAvgTwnPQR59O%2Btx0%2BrPPlFoqiTT394y6THWzAiB%2BXIjqOoI%2BkZtwLAtaH13ASA8gYtvzKAv7YJ5GGQ6J0ir%2FAwgGEAAaDDYzNzQyMzE4MzgwNSIMpwmocaTB0uLSGB6EKtwDGxucYPZLMmBqKyUyjK4t2%2FOwqJFjxTphkaT7GR2wKa9yoLBPrlbph1rCdjdrqfC0zXyHUo0Bj1lW7z8Xlmvoy0o4S%2FP3BaRr1h01zlLPMoeYFxFfXvwuHnlbLKAFT4xARKfeUxJO7bImRp4IpsgoyblmI3jMw13mAs%2BMqu3H6mFvcYsYTW6Fuz2dZtLlMo2JGyHF0XePXTufR5z%2Bf%2FL1CjveTqqNmrt1ElO7mIsk9cFBsrzBn9sdDqvH7I4a7Wyvej1iaM8KHLSZCPVS9vnZ6I1rkUvpmYNpfGa2nvBxatswWrf5VpJ0iCVqXgIrDc%2F9I%2BS3mrPkxP06bNANWLQ%2FBnGvo0c1ct5aSDpAy26tcJQAoG4VFki2amxqNF8lriECVbADti4%2BM21W4wUpr0l2JEPj9igKYd8yA3xMWM4YjvBsl6vAMC7%2BUS6RbMz66AENlH0MFOCzkLLMYIdNbzVUb68FkmdWFg0lr1wHKxowg4hZifRA9%2FA5NID6OvNPJEY1jmXkrb33RyO8XsUsLUMzQCjhKHcB6LX9SR%2BLeP5a8IeCIIJQuX%2FNQG5ex5DIfAIRa8m%2FmOUuui9yUmPNz47s%2B25SMFFtVifFnMfGM0CI%2F0qLsFGhaxvMZyfu3e4wndT%2FyAY6pgF5vztDSx5e5CANht9vnb3JFEhXT6%2BZJOJkKnsnu5vwfLURnwdjdCNG1aQlglnRNJX9BhhpZGg356bFszPbI8G%2FL0M3ZwZFl3HrzOPHonuA2JTkLg1KyUoCcxIsakm6Gs1zsc0F%2BfDHYR0ajcCYI3WtK9nhVyxFBrI3a%2FdA6GMjgJDi8MrWAjC%2FZtZVq%2FIavNLbhlgE7AtkzCrzmQ7465%2B8CapzFE2y&X-Amz-Signature=680cc14122c121bf0996b894525be52caa4cd206290d286e3809c62768bac6a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

