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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VHVTM33%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCPtx%2BlD5vWqEBdU06vUuIF07iuZIqefR9eBK8pCJ2ZAiEArO4NzoeuN96oIDchm9HgV6%2FiDv8f18IMOOt3k5MeXGQqiAQIq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPuxRTtEnlGU7O5mKCrcA4VouSnBofQCgVwisVQpxF3TCfbKzT6u3AGmJthZRoOvcCuii%2BqHLoDWH4CtImL0kewSkNkOBy2we3IRdILoX729eCurjzVXsJNjdmSBxNmlT9Wwfa28JwfpXSY7BIHWuXVJikbC8deaEiMLDOwmwhMreopOQQR0zd6pAKSiYUs84z8r4BcwEf4N6kTAq5KCwkjkF347q%2BlOFs8L5MOhDbFdjdPXCjqR4rr7yIY%2FVk9sDv0V1CpiRyWZr7o1PMjnsvonyg9WUQYfLvnT6DOvgH%2Bf5deZ0Fkl0wzg3pIGdwkqyPvnswANtbIj%2FWKaeqGG6iNOyei5dnBYE36CGk5MXLwHrVU%2FG31GHe7TACocqd4iyGrs1JNX07Lq4i%2FmyucMIn%2FJ72hJl9V6cJVW2U1gbcW9cH9mkQhD94kXJTjEUT3TXWZ2puVNlY%2F3F2tAMOfLPnXP4Agae7wahN3YEQriJE2qOc8fHpLeVmJfW0yuO380DMLm%2BxjFgaYR%2FAHvAG3LDj8NMGE98luFisVlHO1JxqjvHeuotXgQf6ZturBm8PbLONaa3PkU7NH1%2F3IL5qoiVJSO9BCpsAC6bRhMZ5oZwvyZMyZB8H%2Bqp0PnXkOVT%2FoZsfLyvytU6cCemBNGMKnW68gGOqUB32IkhvC8iQDnQ9dS9G7vYXrC1r%2B8P8wc4nCXRwgf5GVOPn5YPlzitdjhUQEcxhfKsxywRz3zJT9LDlzMDb2U%2Fi0cYlKSrb4yQkJGBK%2BD2NA3j4ufQNTUpTbxlBAJNVEYtel8KZEFf5aUmR1gSZ8%2FqIKOdyksuSE%2BpghuIwBfA6OlV5YPmZ7e3WjkkqMAlayxitPa3GvZsbtBc1L1UzuP4CzQQNBQ&X-Amz-Signature=6e3e5957ae2d758e7806fdcf398bbf057a092cf5ee3551985f7c7c16d0672fc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

