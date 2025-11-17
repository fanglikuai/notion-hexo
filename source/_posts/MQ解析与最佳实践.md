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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDYADMJQ%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC4TJ7hoDzROwITxw68pUuaTv1lHuGVRXLE6Ga4uWZGTQIgPFjj6cT4mfb8VQkVaD9m6RIKGTki8dSkqr6XUde3IkcqiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJLVpwKdf5VkFQj0XCrcA1BDFkpePLRD6y01Iwy8lhpIB9pqkHXWu5OqPcvXOLEGeH6cHqJVJAU0elv%2BXWEyrdIHjYa1x0ppodYEUrM6YKERUWFi7sGhHDestA36xGLAR6SMmXMqgiJmZBLUU71bjI6Y9MNVDUUwxvgmgqnLw907q4Fy9nn%2B0mciPQIIaljArEb00jznCNkewZZ14mWE5Q9ambjTgxGCrB3DDIVyyqCwCzPdhAPggklECW0Z1yyZ4ZFjBqLlRS4sS%2F6KClmFH2XwKQWYUZ0nSloau2JBQ30le2h7km0NMpejzH756V7T9jxcBMubvJIMyOTzwD3J%2Bjf%2BhgwU1ThEkZ5GChIQN8hW3HwDZg7Nee5qKMbs85KjiIRvVtRFnXyqWSpilclV6rxN0SpUQsiSeRL0tI6bxiG4Ky2Q65f69VkJJf1272dTGBwjKAF76g709GfpaRXUBk0qAFucJ0mk%2FGETQClVqtL9kofAtUOelUDWDz1fn39kiC7zZGxxCnKt7%2FyLn0%2BZAR1cvQre%2B1ENcbkJaYDnGUEf1G8%2BHU5qpj3OUCL%2FlLVYrpUrVIiLrmRubSUyesIYndGjfDadyHp3zn5VffG2AMy5qP4S59BKWILoiUOXUNQ9ofBsKL9bmAT6bOITMIiw6cgGOqUBuFoA7ypyfsQmzLRYfOkjy3Uy4qgw269iBZqWa3jBFio5aM87kysiH3AoXX7Bg3jzeAJo9LxtXRcPHhZFJLAPa0tHp31RCN4XPc5XQGxiwnSA2TIeLaZSBweYCdlz8ZcXJpVyw%2Bq3LVlVgc6urGMX4yvEJtRfJXuibV2T8R5x300tTtZIi%2BlC6ZdWIuyRqDzeMKTctLiPy2am3yG1VpQPJcUCE9zX&X-Amz-Signature=6ad83040f012fa4a0b56bad4b6c3f7c702ac0b0505b15052a635cbc90d799206&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

