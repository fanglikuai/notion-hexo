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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6QPC5U2%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIEMee20%2BkUOAjnF2ZX2wqstMQkFXx5B3f0INWu0NYZm7AiEAlNQdSAYQTtlA6W0AKZrSMjkeJypt2cZNlwHsxAIw7DIqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDh716Qm2NJOhrvXZSrcA9DpQ8vxuZliumuzzFDknKeq%2Bv2Ul%2BeWr0dcKyatLJIWxBBwoJdYGzCUIxrW1t9f7BpL6Fs55KgaBuGV%2BcDKUJPvpbjS9Jl4eSWgOvFg%2F8%2FwL5xW8KGYv%2FK5dAbTtUR0k2CEreVY%2B1J8liGhGG3lnPA3HhnGYd5XCApN9R3OT7AgSYUQD1HgEyq75%2BUcpPw7%2Bb%2BgRTriMXH5C8%2BZBk9p063PzH914l6qFpgghmpPlD07W6%2BIUL8rMpRNnJeyygpbOhjEkI%2BQxgSaJmaApXC0lrviglhetmXzx2npMJ1248MkNVAhtg%2FMM03ZHr5u4VKLR4FxvVNaXsLpFJMDxi7tWliM37G%2BxpNMfFVhCq9MF4%2BC21bKbinGBqc9tjhnwQv67RCisZ3qrSQNKKiB3bclHtvkEdZ39KR6SYDdUtcPTgbQtz1nDBx0m%2Fl3%2B0UkYriGNuD8X6pZsO1yqaOGs8MVpsztUxRZIHBOSrhOD075Vwo1W%2Bouy%2FL7HDa5ZT08N2NMRHQAlvLwD6erL217kgldbFapBWluUxcIeWD95DX8uC%2BT4a%2Bc%2BAMAOm%2F8ua7YLaM7jRondfoQD765zwYrWdDNAca7BuGNXZVlPJO7lTSOJhhQEqFZPZwmznt87Jx5MNmmv8gGOqUB9rTIWn404T1CiZLJzbDoGD2tUF0KZEDjO6hK%2BwRaf%2BeXDKxyLbVe2%2BmuDRXkZT%2B%2B4POUsT%2Fo9KnRJW0e2HGbeQbXI7xyiBH8VhfFWT0MC1spK37z4tYRxKPK3XWpvCkpGypIQGQVgHVSWN%2FaXlSW0E%2Bt60htcIs5pplTalZCFMwdrTrousFLBfQebYe9zJgNoZWZOxO8QE1p5jC57gUEi5FpDXUB&X-Amz-Signature=2da77bfa62f4894ee38c66dc4f578361dd699078d9434093751b9f29666feaf2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

