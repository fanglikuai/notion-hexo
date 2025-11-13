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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQXNUKRC%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAJejGCG0KSP85kTWEpu9o07bS5sdhUPRHSKw1sP7hjKAiB3qCJlYyFUq24ERa5Edi%2BWuUDFaL8Q3DlhNzYndbK5fSr%2FAwhVEAAaDDYzNzQyMzE4MzgwNSIM8KNUBrP8RboYVuo%2BKtwDplDZZxfEHI%2B%2FNzZjuEfgE1TF9QChy5pLzXRn3GB30hlCVfdD8zZba574Ts8Zys5xMp8ndm6UyBWGmaD%2FFtEjR8IAk92yH3tJwkaetZB3hyeREr7a2eW7x60t2cJtqgNtM1E8UFVbfYC1lnWzEpriq%2F2%2B10vlfdVSc5z7DOKA3bnUtXUrC7uu%2FDVXgCvbOnfUouPZKeZqMQ6yrJ58gtoyLuVkcAJ4RtWW4ecylhCuAzoHLZCYp%2BCgl3J3t2T35F%2FNt60VxWevpXuKsHwJkdyeuHm%2Bk%2BEnnvgpHpdg43vy44SLhgIGERQ8%2FWnaMrbvqX%2Fusn4nagNatsg3h6thx0rAHIwvVRjFQg8IDfQV8nlTyzuzs25JIX7OoeXlx6AhdqzctgIblaOMAFjNMYWvx1qYuvAKtMZU7NXZCzVldpyZCTmCNPV0KtsnOiVB0y4l6JEbJlHctLEAccwwtZMamhoz82UG1GCJB5Lk7GECEWM5zJt7T6LFZ9eeVTbdYnZ7fIqV4YRSl0IIVFlOuQUCoYHhQK0IycUrXdPNud9fBmx1NgpVOoeyk3%2B1ggctt0k6EVeNrbQIFcKgVJOXhH9T08Yxanq0EG7fB8h%2FuKDeegW1xyzODFqej9MJ3lhgTwQw7PDYyAY6pgGhtt%2BRcwjHT0JoYWFySzIvdijWSJ%2FCE%2FKkXk63kC8A7Ml9OQQhwX5JL%2BX%2B9UDdY7u4KVIndU5JI%2F3J0V%2F3LIMiW9ThcArhZ15UY5wo%2Bm2kQ7I%2BJCUosIS7S4Jo%2B6RMVKMdTHYzqMJj6br%2FF6RfWTNJDKrtESRp9p121nduEANsWgmkAD%2FR3jXDVKxUms3%2B8jrbeGTcXO0M3oDWVIVg26G2phq32rWq&X-Amz-Signature=5426cc250d82eb8f004ac0d98e2981901a3e7b8876008b4898bff7f069844bed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

