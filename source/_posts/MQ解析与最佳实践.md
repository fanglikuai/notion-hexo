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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WYFWDE7%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQDYkXq8zpBvPCnXoHImTc%2FbPcZWLg3qdPW8BMyH%2BwQXMgIhAKjZd5fZXb1ohOHIUTBAjNsTPK2RBn2mXdFETyWsE2M9KogECLH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyQgiwQt4xE4ZG%2B1jIq3AOJ6FtyuKzEUqaPyHMLNDofGdERUiqKrshfTWrIOLG%2FD6b1H48uwRMu2WKL1zkOY3NyMto7zTnPCIDODb65ZEbcexILioqc%2Bt3Xke35WCazNmF7W%2FiBV2%2FHCDdd%2Fx9KPcRJ6yMUr%2FN2I5tU6au%2FcERN%2FDEJvuKmXN1UYa7t4zUxjQBP60XjndRxsfztqHA5O4TwDB9uMg9RqlJl5Vu2GKoxhwSMAoHwmf3AQWwTYKKnQThJ8RZ4KQd20XGpu08OtY%2FP3l5hOkCdhMOkoIfi82Jc6%2FIdfkg94JAeZsxfUoJukYooF8XYZlKelPWvEupBGHawbeBFEfL%2B%2BWn4ORFyRpQN9i0G3seR8tGlXJ6U2C5nrtHnZOxf08oDxcCBOiAnyT78%2FpvWorcJvhV%2BaBpPm8QUa24Nyyq9ghXHNMUn906iVJgKOQjWjkfBQnPgCVyp6980FPHt5IDwkqRHp04m8rxhRUCZb2CxppZ%2BTjzjvLh5Oozrzb9o2J0EqqY%2FL8CTQN0g266v35lkcsYom7aqfQMLyPKC5G5q2tWNANDiqvL7XA89nHFYAjnZv4pMg%2B3VhwUtHNC5%2BNTkbhW88saVeVqnF1AmzZ2SNCV9z7Vus9marU%2B0HNOqFtAzMMfVnDD4ma3GBjqkATx1duuoGEkjSWu%2F9AAghAtTUxF79xqgR7OlXLtx%2F4Z8Y2iTeYqQch0TIQaQakgYNXm6%2F86rFTOWrjpChOvnHGz49ZeynruI5nATeOsdRm5GY2xZId6S0p0yLoszs%2BUDWe5fMefZ6AruuiYGML0u3SQVdCMi9sOKsXx%2F1xBee3ts%2FhhqHLR0JF9ApwZ%2FC%2FZR316Asf%2FATVg5e%2F6mgPVXJqffTSFu&X-Amz-Signature=6c101c777f01f63286ca1e9413b59da87b0e1518d35f50e1c28e0f5bc61a0fde&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

