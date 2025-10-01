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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IX7LHLG%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDw5DVeWCs3s6MaqR%2FDa5meA0TOYv5o0uG5TH9dKF6EyAIgaC17yyrIzd9U2vM0iEQD3%2Bqpvdl0ojz5Oq2dfcUE0sQq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDDUIjI0Glz0kEMMJ7ircA2FqjQ%2Fw0wCMjaRN9q2krv%2Bgi2XGerCYzt3Onh2jVyV3X298bOE4IgIyyvnd1b7cfdTVZ9SKonIxvgLIYGv9FV0cVeleHXKPpZHmDenkqaPLPfknSwTYAeeDBcY1lDKLgERImVV91cG70lGYKJdR%2FeSthRxLypdveUxGqTNsrEpJd%2F2pNCOOatPR%2BzcLtyPhzkqbq3pHMniygYBCutFe8%2BjplsNSXjLEr7uX1IC7qydb%2FU249Kr7lrXwdxXK4smN1pDyb%2B55jruMR%2FXcOMFEwHoCrQLcl6ic%2BlX606kJ6LqPKmpoxmrLV8Ll12sBtcI%2BdswIbaAL9v1d63vbgf1YaNmBT%2BB5JlJF7VWXT8C0qaFhYz2%2F25pg8ZCUVY4nVcILIxjiYSUai0%2FJrjFVQybHlDxVju8cSmk3sT0FFXm6W30tReOqCZxhfuw3AQ0vSDfaTbUrR2VrILOvIjmqwA4eOB%2Bz9qDFoiL2xSsXngPT5wg%2Fs0pn%2FAWv1T6DKswmcF6VR8zEkE9Tre%2Fm8mKYz14hw7CQ7CRNS3tFUam3zzpH%2BLQ1tOT4A12dvtIXtk%2BoWBSSNKz9abbc%2BS7NxJyR0Cy0FCNACkjMbx0VC7MaQBbVlm6O6toru3wPumI7C6RxMKiy9cYGOqUBG4MUqYUP2iwdsjnFD9MXZ8EiYfg5yTgOvPDYdMhrZoHGkL1HAi215TKmN%2BM6wUOkD6RytYrqGhwVQo1GjPXE91X6vwF3QmjyhULFajtxLfmG7ImhJL8dX4IpuPu4k7YZtGZ3GmPm8bszjZ1oxLgIm%2FpJednISElfcEFtug0p%2BGzr9Da%2BYGz6RLPUGH1IGF4OGmoAlJFjkQ%2FXtvsjQPKbIHM89TUa&X-Amz-Signature=96d84c25b16f3dfcc6986668210a2ee03b75fd03fcc49a4f4c64a21aaa914231&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

