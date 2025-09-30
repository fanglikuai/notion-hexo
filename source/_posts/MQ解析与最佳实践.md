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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WW4P36NG%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQDpyjcx2fOwgBxFt%2B5NBsN5g5ds8OFMIZjm9pSh0fDoxwIhAO5qzFoXreHUdTcBF03%2F2%2F1zWffK2ZGnbUX0O1qGg7hkKogECO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx8Q7d8E9RPdtlgo%2FUq3AMwUDk2TWq8FKVIi9HInALS6Ib5Mid2R%2FoSiodi8n0PazTxwsfX6ApFSe8LGZ0FXcttYOJ0VwB7jOyAif1UkUTefjNVreSXKuHCzKVR5wSfp%2FgHOB6FjrEl4zpyz7ueoSUH60uy3OxOEw6VC3hJVgonuzAIJc0tZE%2BnAfqWTj6Vyfyqi1y0yeDTKKG1%2BlmQFFuyWvCUlnfgKdVn0qCXXtu1lOZoCvzuIHJmZzCnYXuAXBqrSgDKFJxEHyX1hBAkhatdP0qiKmIKyd1OlAHUjyY%2FB4DDPnu3Rsbi57pT%2BLkeyKws9JuxWOxUAi%2F61%2FnQj66pT54ZT3ybNbcYBNOHzS%2BtkZDcjRzB%2FvXkHo%2FjlOQdjiWQr6Rw8wUInK%2FijL8c1JNuPA8b2j%2F1M6H2Yzun90WqxDmQablCrnqEiwfqhA%2BogWrKgQFer%2FctLaXWoCG3B5MvUYOT37kpxOyE0iHkZtEmv7exfni1hKxy3XM6qy2dGq89%2FzjmK5wEb1%2BA5GCdafnDB2KTB0XlsfqZIZQ6aRb0v3l8PdF4LLdvL1j9pfzo%2FG61fH7wTVM4LWrnFE9usFpQCRdllsAcvlV6dizNc9xcXJFax9nXp9Y0VrKKscQQFQY9BnOtYa9a7Z1sxjDsiu%2FGBjqkAVt3xXflobQl910OzdV6PY2Mh2ooG4CfH%2BABIODNueXHuPZze9z3J0AtlSqtu%2BeyBEkZkKLymB1feS%2Bq2vVIkMrMQUqiJ3n15cxIqf76B7TijfsHY8Sn4xkMO2JFzX5Qdo6nE4IVrDBpagEeZnlbtitNqW3%2BUT8zxp7wlxZJU2eI38%2FxNsPdozEWmXd69wuaiQodi6amITdp%2FJsMrX3xHSWSwPbh&X-Amz-Signature=e2ec76cc7ae620cfe858313a76deeacc927f239fa038468848e41350d084a5a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

