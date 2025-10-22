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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QOO4YDR%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQCW8%2BSDHK6VPW%2BTt7c2hDkqvsxEZgJotc3cT22%2BfzvMrAIga6MJqOqHfyo0JvoiaaHSNuhlJXPtWrExbpI7X7PeP5Uq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDPmrXfMer22NRGpmByrcAxM74MabOKtFANVti6xJeCcko57amjW%2BoRh0IpvzVvcQAbaSSKfa%2F%2BrmvZf2hPKdG%2Bx240R%2FZ57QcpZg4RZOEgAZOMDGDv1MT7PETdBkmo5LLKU7lipxiJuqMfY49RsjPA1UCSLtszO%2BpuLUNX7keUjdh3VvnXKgBBtVsUHtloqTTg1ZVgkvAxsDh0Elj0eyVGPkI3O%2Byd%2BjLU3COfGGBLy09%2FIOiJFEUuEq2K2zErUpsmyGaF8OKtXfbw%2B%2F6elqKrJawuJSWpwl4C4R88ba0Od6NcdQVu%2BcC9evgLZuPAge0gFPfXhhCDaDCIzYDyutGB2ysDtuba9PPI%2F4wh5DEImMe%2Bmu7F%2B3MPeT5mWEYqd3dQJMFcYeJt7Kd4iNJaIc7WgEkM5Fv4FTOdBX851B%2FAv1afnKC5muA1TqJ%2B48f0KOxYsznKLrGxSebo4pEN0nD9lIHTbre59t57bBcg%2BVmERcvmn9Bo0eYRQfecd5Uf%2FjVfSIs9%2FYdH%2FUgwi4MLx%2Bt8tlU2Y0kEyZ51E7Wn%2FrLVdlKrBLyRGLdxDru5fI8fVjFFAqxv7YJ4Mi7Mw5cx46ZooTv2fZ21YfPjt%2Fc%2BiOnMY9lPHPKaQhs7wE3go8kF2aeMB5ru7o8Pk%2FOEOBMMm65ccGOqUBLbcCtN0%2F0%2BUa5HzZWbFJJbtXbcf4ahhda4VZy%2BiGjAzu7XqDU2w4QpUmVdmlPVAHP09RKzPIvw%2Fj5w3ZzJJqFl30Aq6MAer44SyOwslvoSRtrHcSmCoS8utbLk1HvlZAGTf43GVRM3VAd8GBENdT%2FGHjflF%2Bv%2BRC48Zrr0l5QGUYAhzJwtkRcWfS2TKqRA%2FBTxNewons2OB5jSpEPM2dXzI4esw7&X-Amz-Signature=7ae272d8e31b6e9f70d7e35156ed08f238b2d358702ae34bd90c0fd35e52ad3f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

