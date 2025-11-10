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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662EE52REB%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJGMEQCIFaErhx4Yf8nZrhUtoFHtxdiq58TTDkEVlRjrvzEu9JGAiBQ4NC5w6Px9Qi7Y43VskLMtH12w1gHA16IGdRtazMQECqIBAj3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3T83upY0x9fYejfNKtwDh5X1bkJkkTa419Ckkvp97orXW2lPLAhsb%2BVraM1muWsFRpLiEzDLOnsd%2FGWoP14QOoWzW8kVJ8nEQ2ZeYKMCeLNPzDd7h3M8Y8zhjVfE9aBUjytGMoYgvmR%2FoHVa1%2F2KCQwZUTVpD7LFMx3sQ7uPfMyYnM2zvxO5z050sB8M6l9FP4MdJrqTZp2h3nUieX1zoCHEzw5YB1dxrd%2BkkNIH2KZUXD8xZWaLlVEZ8OmEzZtgQiKlBzaptYSL%2F4a%2BIbAX4HtjSNsrK3rs1YjJ%2FeEJCn0H8YC9LQIhkigupAmFaF%2BBOmdSuCgIgqAHt7gSMMhxO919cK38sbOyri%2FmlN3Lv3HCZ32eAd69V4I5kSTLQi5pdvWSZBoyVyF4TsxRvFOPtVw78FyufYgOInQL7WES0oGW127rc1K7zpD9Chg3Ixq0TyxnO3%2FCYlsLNJ5tyL0jRgRhTkEBidf0DMZxkZwrXSzhKP0bJwXQcAMcfQD8VlYefCwXlh6aEcy%2BSYqSKKm%2BW1uTFC1fICS58h0OoSiPEiAuBJU9%2BZaPrsW5P92U8o8WvDv7GZx8yaLUuGsO2KgOHtM0erAdT897xSn6s3GOTYt5d6TRaG2LpQbzNCCUTdvfIULQnzE4PgrIIWswmaDEyAY6pgHLOjmcRKEzY5Z%2B6MqTB7MLtmzUpt%2FZdu3a9Z00ODUjFQ%2FZAEGfAVNzmA0%2FxiYqFwZll08uFKSGz0asu6fDhr%2FoujFMzKpOOvqSqqP9MnceWL6l7Voy88LaP4jH2UFfL%2F3RK%2FUrW7EU%2Bjuk%2BgMJXwTYk1Rh%2BfasCM63z4Bss3d2Aign3VZh57jWQN3HBvgxKzE4rC2h%2BWYEKSdtRTpTt18R4BmTZ8Uj&X-Amz-Signature=5e764190b0e3bdb035cdd9c621283491b19c50289dd3637b50b97ed090deb000&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

