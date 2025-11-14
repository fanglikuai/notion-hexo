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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667JEW2566%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCG%2FKagh2jm%2F%2FookKHaXPTyMDWodSj9tP%2FYbQ%2BDnKvYJAIhAORH%2FlSGN6KRw6GHWuoafpRdCCe%2BrqqYJ72Ap0Pqcr6gKv8DCGEQABoMNjM3NDIzMTgzODA1Igwv5hpx91cshR1cZZoq3APo7pnBXZe3HQzpbUFs8y4mIso9ISqgq44vz%2F2UuXL1jD1tvtCYTVXVdVbT83gB%2FR5Aufc0wKQGM42rizov5iRWfvlh2uYZPrqDgQfVgZtppsQF9qDBDw44ZQDKxX9uCDmqwB2lPRSJJo4nnFTaXypFo0GLG0HTV8Ih3cGPaTPAh1pp3Evp%2FPManqlWh4Zc8d9A0jYxDKIXDUFcGs7LsnIaaz0L6g9c0zXU1n39zFYUVvztJ8oDIcZsaipZB2O32VI64Di8ArTtsmBb1qgmZFUEhYoyhOxo%2FCxoUKnOJNRs1NYE6BNGwygt1Br%2BKAq4CGRJpKTEwyCwbI%2B19d2Xuq%2FLMiYcUL6dN0v1k0hrt3Air%2FqFv4OPJ2XSe7VpQLhuUUkvlnzI3AYPflkmzPaiyXyG8sKigMFL6OkSCo8hFlxdAFQxu326aMHnd0MraIkHWkCwOS2RMx7ayONkuK3UvdpZliDXts%2BHbRws6tBkLG2zTe2TC9DEIeDyMi8%2BTLV%2FzuTAfH1iZwHFWai%2BVoNyvGL%2BIdBWQJQjAfFpqj%2B1K6bUrylayWmDQ7Es730aU5nIGdGKP10woG5gGGOdrU6eRU3z8P9VDJ0Uo3XphjtNV09kOhRByul8FqjxyQcfTDCIwdvIBjqkAc2uoER%2FEJd49sDb7zJiIFWktxYfgLMJXf1YwcuDTWeFGUFAJN2CiPNrEwKYLsRxkzqPqTQySLJ8cPCSNf1fWSmNAneyTAI%2BIbTrIIaPg4J%2FLuQyiO3vzE6sfsQ9CRl24m8s9K54K8QZT4SJ%2FTkzWiaOd6BIc%2BiwISx9r8wuODuuH0BdwnksHg%2BXQ2iK6QpByYNOVT%2FS9chAQIB%2F7Xx0ptsq1dt7&X-Amz-Signature=e149cdd2a8634a902e0ab0528076ab88c2fbbbfbcfa604c18356353de4312cc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

