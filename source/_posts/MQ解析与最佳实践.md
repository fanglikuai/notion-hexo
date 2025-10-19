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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2Y24KWV%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQDp%2Bprxy212ttST0uSE3%2BxHKCP4BGVEd3NblYFCaT%2BNJwIhAMREuwygimf8%2B2hMldmqvQiXO3F8LGU3jZFfRsKcpbChKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzASGo9whaROlly9GIq3AOpu9ZFLs9TNJTHIbhRyRO%2FFY7kCCUZTpn5qqr8aDMSVLrLkEKcb8uprfz5ufefgy8K63ulqfXUI1YmuP1NPFqqKSQ10zEXN3mHfQTFuzwmKPE3L7IAsLMuc%2F1cAS30eFrGcHUnCfaPhE3ydmp0K0mpYBXFRtWxBbFJX7DMANWGoGO4Q1ABvMXU%2BJyb%2FiRSZKkZWGlJdehJS%2F5B0dDSQ1gdy8YvU52FxT5IvVfMYhIQyyIrsIzQUXuoGl3xygQukaxpypgTjnJomxg5UgYdbrBz453iaJXrfsUrJJonfe3t4jpjJj%2F%2FAqY5jiL35PwIOx3vSsphjSqn5K1iqRu0Ab4fho46fXhFXyamI%2BmHjHdfAHo6HyzbEd906Y%2BWv%2BQAC%2FRXobYA297JdTX%2Bxxy4zSH8KccEIiiA6F%2FBx%2BLRbO727tkHBVEFbYxPHqSh54NhOjAhY5yBBCJ6WYHZ%2B1V4wcQvpH%2BX%2BJ8XhQEvAKqJkc20GEhfRs%2FQYYTffyLzyWZRwOJK3ikg06zrT3pW%2B3Tf2ovtLrjA%2BTINvgnBIkvvqneSpFAmhN1dCUpB5Anqp83UsuLPOZQi2B0ZpS7835vPFZiDManWnxrvEX07tSEmZOeT7a3VgoKdRiq8GXf7UjDk1tTHBjqkAWxglhCfUgra36m4fPoVWTr0gB0V7pF8f%2FIALv2ZbgC0lXGez6lNcbrYsaAU5tUn2TiiKy4CAHteg7vZeEGNrd87ayAONb%2Bp6ko%2BNWzWHvF6CB1e%2FRp7Dg3AghReiAVfVOCJa5VqCoq8DswwwLAJSg%2B82N%2BUFu%2FQ8%2FxgfY%2FpXsa4nDJVvKWKICsqR4y4Q%2Fnv3tcUsHw3Jg2OZMEsHBwmAYVtFe3G&X-Amz-Signature=6bbb1ace273f58dec0d7e94c20c63b153db25a0c496c03c1013b06a36492df96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

