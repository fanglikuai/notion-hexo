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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TD7I3COD%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJHMEUCIAXb9hd0Wr6KhClvz6kaD7A2VIs5D4GOZlAx3zJWo7hQAiEA4nQ0inw2Nh455Pl0eQ%2FiI1dxk%2FX4NyE5isUsnE%2B5tBcq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDCqQtq8IVPg37W%2FHzCrcA2tmUKIP9uCkofkppnjnihkTzv2alZ00VdEDK2ciUvGZLofZtbjA6oC7%2F5pYdNRFxzcI5vsgupziecbvgUytmCiKcZoMe2xaK7SQ3OCdu%2B6C8sdvTsPG7CipFeIpBPvrfr2kbwhda2i3vfhJy9C27pcKOhrLhahi%2Ffu2i1xwVwYAh8tGQRjnV24NwxWNAg5A%2FwFBCsWA8EbRAPS8i5y0v0qlwt07l5sL98ndGxvVAGGf7yULgo%2Fg%2FrAxt9k7ZKLhay3VqPvXjFX6d22xG8%2F64n4cJvbywvhnrC%2BTysCXDCu8srn0S90SwEoJd0rKH6s4arcnwMhkI%2B9868L%2BgX6uUz20zcwgTy5ZW9aaWw1IXdAyi2Kd7iLiDQ%2Fr7gbw7OwEpiqtbuEbrQc1yoYKqIdyD3Q6RztljgzzfimQXgb2I012lHCcSQU5N3QSYoOn5hXWdRheyWRZqewIDxZogy9T9hyXpIE7r9BW8sTrDdCPgEyztAIyO4arSPrs%2FqEkK0Fskg5wU8Zv241z6pHdT1nX2kSWKgqPBnPwO76V0nxMwTyLbd85zO5jTTTlKQe30FhHBfWMFxuRJoxnINqHkIbxyRC4CjzSMy%2Bp7zBBcH17VuosQL6IQmNcm1MQ%2Bz5oMPf45McGOqUBoYDcAyt8vBtpDnufydpgBmf06iDKkJ%2BwG4vDzg3IQsVK4XPOyNzE8ENSrSxAQXSnHzl0klO6S%2Ft6GiLN53WJr3uwgsVnIU3qVIF0tcRnTwxPseauWCJqSzIGlcYQXAEF1X7kocz%2FTlnPQqUv4p5l3C39%2BdpZ%2FYkuKsZfMB9O9U%2FvkhO2dfPRWVeWihLEHb2Qas3PAiYvpwtuhOCkpv84Y7wHt9Wp&X-Amz-Signature=e35efeb6a52e52e9fd7598fcabdbd84e00f75545f59862ab91112888489842a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

