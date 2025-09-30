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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVLHUW6C%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T190100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQDIHMsplWinLve56JTLiJC6LKP%2FtnjUJEqFs9KnujzEIAIgNIElJJvaePywQSGRsDNb5dOaJ%2BI4AJjPzFWeQgwzUvoqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJW9cPhXP5GA5b2u%2FyrcA5S4wZ1IoQoa1WeljrxO5J8%2BXgv1eVeS1jgcywwokqrj4b5cTsOo9XMOBr4YwPilEdRC5m0QuQsJqv%2B5WuR5kbFOaeO4aVNB3b%2FWtyitggZADCamYuEL%2B1XeK1VxhtYOGRTP5teUi88VGkkaCN1zAFXjd%2BA1YOoqRLJfiE8Cs9rUVv6WVLsjPL6M%2Bzu3dsfThhRSEo6%2FcEEBUVw6hIDbZoqB8t6nnHhEljau6oeNWZ9Qo4ggWhuowU8lCaUH5Cn%2FBu7FHKSwk9odbwV%2BVJK0h55IrTtLBBoeiVi0xUY4let25Um2ENxxq6tGl8%2BRHFO7VK6zNj24hqLY0ZAd0Gi2OEZu4EQNdfTgIugr1BinJTiMpMYjO3zNWsxdiLdF8wSFJTHIpsqL6VwZQVC3M4ksZc080lslNvy2tFMFs5TCc%2BL%2BoXfNDFO5DX2Xa4qW98rLgqK74%2B8yaS%2Ffor2K7pejX650NUcXnKTbMsuvpE2cGFX1CgrOpZniC4q97JuCkopaP9CsjeMNgoKsRoxukiiVvA96ploQY3uMkRuYYbKqmo5L3Khhk4GOk9XWhoTdTlB9Xreh3JFoqLcJ2nALGUX2wdLp53raYa0QuQlHfxi0xJgb8fvnXJhQc%2F9eiyNUMNe38MYGOqUBZhkezkw1VB9mM5uXbeBeNXOmRO8W7TRFB6mOzfx9UiJyxc3Uh3hGAP%2FyuDdfIUVwattbYl2sIK0M1nEDwwpes%2FaXtmwE491jtHjt4erfQnM54e5APFmFZDJTJy%2BCDIlYMHofT4O4bAFYIVY8RmW1qkuI%2BWQOtarlYxiN8amcHBpSg74Pt6WPjExfGSYg1PqTdgIDZ9AA32u42UTQEqhZw7QDo05w&X-Amz-Signature=26f8fa17b94f784fc25bb11840f528bdd8238de59880d17fade42d82b96e67df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

