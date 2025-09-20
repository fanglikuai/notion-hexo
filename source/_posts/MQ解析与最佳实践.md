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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663H2IG4S2%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T030038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJGMEQCICKaFNvoEbLvRN6Fq1PY32pYY6EAcZofdM5SPv2SjBJlAiBrwCb7qlbd0lUzwFWPjh2kfHufkBJz0wMWZVu6J6Jq%2BSqIBAjk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbUfxnmnRUbMj3Xe4KtwD3BQPtRHAwNG8VvHiB4PiItbpvvU5iF0efsN5JqTo93tPjcGzC1TzJRG0yPOr6GxFO7To%2FlCbEoB62Ou49oDoHnl0sMi3ZQoVDyPbO5ikTS7cyceaon%2FvVWW6umnyNk0Jql7DSSOZMa%2BctG2bltMJypWvcaMSjov7k9L70uRaJti3%2BNCN8qShfRfBJ0Li4ivpUZUMHmdtOeg1B%2BKfDL2ym%2FNtNP4XkFtuSy8ybo4zk5vUHzocHl3Rgig5thbzF9b2l5Lp8m6x%2FL0RlanaoI9Xk0cCTy3MEibSBH3wdYeCiOWSlomtjloXc2k1B%2FkfPFArmlpAaVqrxp31ZPCK5o5%2BCmO9zavbmiU20R0lQyGRQox93%2BKust6x47Lfrws8Dd63EKtbLGjQtQYke7cRzLPIiCRvc6dBMPZ8yRin10QCiCwVSEfUZw5nDbyXt56QtjsmKvo38QOTYgOH9NGhU9BcWV0E%2FrMJ6WCwUbHlMewEIjFP24hDZ8EtSgTR%2F13q2hMO1PqkWNaCjJ%2BA%2Fr7VgrGnZcX6KRX7HEqvnKP5Ni3Xrx%2FdHGuIqGQuawpoU59p8zZQHH35ewp2ZxcImeBwXBpyH0%2Bszby0W%2FcCKZ8jRlP14f9zXadwI1bQZFKzlwwwwam4xgY6pgHqt%2BscZqaZs5OEY5leFWEWF7OPeIbif3YAsOiFX7D1VdaPnT4hkqe0gyF62t%2F9uLLRVg7OLrenxwV%2BbFKz1fnPA3lzo2I%2B7mfLiONIaQvj13xhfhtrBHUigZoP8XPAXf4oMRB7seQeA10XFC5xwd1hKXY6RRNgGt8h%2BkAS7RiMk8Jg3qXPARBo40RMSbxAUunEiRnJ9IBPfk%2BQQAj17P7Dq64t9BNp&X-Amz-Signature=2ce2ddc166c373f5b485c4a29540c1ee8338f6b21015895d00866888494090bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

