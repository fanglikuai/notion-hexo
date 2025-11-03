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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624JJVYUJ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCXVJiZgZPSrKnZMUpIZqSkfsz8QI7DDAlYEMlYSc%2BF6wIgcZPmJhShoEjr%2FVnLmUQGy0Iknx2aNMQbjuHXcm91C3sq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDJ2oyQwgSQ%2FYnUMpdircA%2Bo%2B3Gccp%2BBPU8Q26FvdmC4T%2BAO6o4jM61sKnD2N07CqbM9ziIDnxRspjCogdxcR1Dtv%2BEZlRzARPKii2iIRZsb8O%2F2z8lwMlN6WprtozkhG212X0CT%2Bok7rkQX2ymYGZvCEJKDavSlCEJAd%2BZJGiqAwkFl%2B6OlDaAOV3QybHVxKzwaOaMPP3Ai7UqBMU63cnYPtcEf7V0L0uu6smw8yeZZVO0uma5k7t2rirU8SXcOKemnampvX8Kvw9S%2FNdAvpz1Rt4d39HXNVk5NzuyOOGQ3dPeQ5rY5TBX1uhRfqme9cMncPzs9kx1X%2BvrOh76vjxVn0QClI%2F%2FG3BHfqnbZQ1tQgJhZVIrn9vy0nkT9P5BFMZWId3Ud%2BxvxX2U%2FKYBJJwyeEM9wKIcVSFm8Ns%2Fy1EMMia2RaGFxsq5RtPRQl49mxfkZ8esBFWVVa9Gnec2YGqOjwFbrFthwTv3rFiNejG7fHIFw4hlLTOaWH9Eegue5U9JpZOlNNgyKNalKC3wM5mLgXzCsyjKBPth7NyXpuiqs9hPJAeslSjDycMQuvSSdzuXE7IwceW3iMEnJtG2Sswb0q9I8NQJDVIN9OegV8fm9d1NNBS4MN6HY7DzPn7rqtT1HakS250xGONQDxMJW7n8gGOqUB5AxnQQqBSeg8DUcYZeaCwSuNjUMWQJn76C7Gs8wItBiPiGx7JJG%2FA2FV%2BidktxG60ylNDLw188e%2FYZ7aais3P72b%2FhoCqSHZFJ5sQ9n1nAsneCjLvQBOC8BceT6YNmKf8WOo4%2BCF2%2B%2BJJzbkdnneEVV1nNo%2FkqFTNDTaUODy2O1uZKzcP%2F6B4ricpvMSsOO0Zey5o6qbe9NedRnYWLdlB3rQC40d&X-Amz-Signature=f236d81005a05e203815f531b89caebc7f549b16ecb6a5a6980eb2b57d9b88bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

