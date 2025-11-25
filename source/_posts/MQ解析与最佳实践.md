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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCZCBOAC%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDKLWd%2FXpfOgmOT2hxKeAQg7R2IJ%2BcjMU1ZAzuQ4eRvGgIhAJlD9B6JXCcH2GkRl3vFgVvCVLOvpypNDguoKGaZJTX7Kv8DCGMQABoMNjM3NDIzMTgzODA1IgzEFLQs9dVO2tPtUU4q3APE%2F4hhz4szsxmsH5KIdZAktmL79mvq1NUbOpBt9ZOFQHhRAAeOmKrwzsD9WG9RwFu2DCmbn6CZZ5nUuaCf8v4aKQL90JDrtG9%2Fq0Kz%2FSTsrNkH4Yb1f7JebDIgy2Dqknku77hRdFvP3%2FMnF%2BlmpC0HTME0kyF4kWPzTmw%2BHQDVrbwMI6NZJFN9%2FetaLMU5wr8W2x%2BbtEYyUse8tIfEJ7W%2F89TjT8jkrv7KrOW88PYwrWwufec1MKK5rLkvCNNe7KJAEdSIJ1bAlFyMqtWowvbSGy8MFAlZvRTJhkffvNLkVbczozEwcVlMRSHKV4M6VzLicoejehZS7Kx7%2F24hCEhwMage4esM%2BeOG6xh18tSW9wTsFUgtbAZs2wP%2BSx3vZ0IZfXJZRp9Gt5z5N6qrl6XbQseOgkhm2rLk9O2o0G04s1u3rdWUpJtzPilE6aRYvE6qd6BcMAtNW%2F3PGtF5HXm1tSmymX3q1u6m4%2F6AnkClsGHljH3OVg%2Fz6rsimavcXNEZyY9uEFqaEpSTjSeLuJy%2B3AoG7OvxrOP3jT56xDu3601y3%2BsoYCxj6MqwkT0uyH3y7Zu62vT1ejNmmnIVYpWrwynApQcgNnakke89YdbdI2AIiVMIF7jktQVtOzDwmJTJBjqkAaPVhz%2FIH9j5neDtFBNG%2BS3WrJ%2BFdhYBKTxcLHae4jA93T75sVd7ht6ZAyMAHS5XC%2FNuXe%2Fm58YU0RM3FDP9Fc8eb1L0RNW7XdJnwQ9JcaiGrbU8w4QtLG7in1MofoXKgEkITX5EH91M%2FyUSicVLMUXLt3EvyX7WyegCdDazS%2BOkgSJRjtt4lRbQnfig4Is75R2VQ7NqWe4HRPQNJxx%2FGrJLM7ms&X-Amz-Signature=dfa6ce6fc3a00405309236ef80c9f233ede65b22a7a31958f6fe2ff96b646392&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

