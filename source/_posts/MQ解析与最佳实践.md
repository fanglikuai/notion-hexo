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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJATL76J%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T060036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIF7SN%2F%2FTOGhQrFhSPU6f%2F%2BV14kdegzCCVbqxCyq%2Fm02cAiEAzpNoXy4SVkybzLOUlplsNos4WipbAfVOYWa2KvfwapMqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEdFSuInq1rxyCJ1AyrcA0cAxKIATFsaQVKltVb1uLyObCzckGmgHBzv7VDSc8fdR8nEAJGrqDaUbKZQv7L88IjMulT%2BdHHiyyzEluI6xODQIPg%2FLmNTyQHcM9w2E8p7QMkUGt4L%2BihHs1QTmqSu1UXjV3AQoAspjVBtjBCaMmrfNSad32AYTU957A87Xv81%2FojgcY8zCUEQ5GQaxlE2DCi9YA0njGBIi0NVK62cmBOwZLffqQUVBT%2B3q8utxcWbqUnmIva99vQPvUxaUfGcmBKhM0M2vBan9D4%2BctsyreI4aK6jO%2FBSacGZLmhl4S8KnKrjgqEeAzwaSdMwzMDc7jexgopMOqXv%2FjvLmy%2B3NpdiwGQ%2F95S6qiw1keIDhSF8mxWCFQqcc9yhSSnEB0lNXO7UfAn0vHvI9KYrHLcyl04tnkdUc6WIfleuy6ekHcxWJ8NZKVsMvPypv70saQ9tr%2Faf7KP5g7cI1X%2B6B8%2FQd3hm6MvM2HFBC4IvMgAybdPLz4JOwubOtEEr9RQgmaTpemPNR5BMtxERwuA0zpJB6buFdqnQ6zo2nBxLphGXKNWYFMxu2n7YriYKRfzgSt81Kc7VSSUeRn4LkAX558xUjhGfQNh2%2FblDgzGh67HiTGl8E8JeTbZJgF%2Fc9JggMN7vi8gGOqUBOgm0NHsYsbju21RVrJHQRE23E6xFKLjO%2BxHts2rgL1KJash%2BmTqcpEAfnCVHdwbbaz75THxFFsH3xryGp4kWqaW1hwUd3X56t3NpzeHeAmSFXsqhbUglK6iwD1MnG3MQg5bacuIv7d51u0Gb8f6G6XnZXdTCaTtn9ODudCruMTunjvDYIRuQrYlB%2FB5tjEXUtsC%2BLV6ULQpCR5uXt8Q9LG%2Fyp1Qj&X-Amz-Signature=22ed3e1f77c6bc6ad0c19fb35762731ffb14035d790f202bb44daa293c3fde71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

