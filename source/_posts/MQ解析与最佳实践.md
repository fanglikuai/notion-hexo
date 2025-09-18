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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665B3IXJSZ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJFMEMCH1lvxEaUkD3qri6zbfywMFH7WbabStkMC%2FhY9is%2FW4kCIGW5ZbFl8Qv17%2BYHrw8GhmQQIRuMoPldfEDAHTRIhD80KogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwuKTJ7xiGJYC8HhQgq3APG%2Bv9YYwQKZlgd2ABMUG%2BrxSjTJs8vvnkdipHU3DqkmTSvBQXv1YPdaKaq2uxNkF4rfx7D9jUM%2Fdz35NUUJwVwOh%2FvfN%2Bv3C%2FUWgNOEZwF8LElVtdGjZygWxJG%2BTGF3dkFyRyIT5pY3uUyy4glnsZafu3YYULvns0OVESnEicG4%2FQpZx5X8dWEVzbK5SD1tSXQ2LrrG43R0mf5vtz6bBHA1bqxydHDQhJ42AMZlAGQx1A1gTM8qr0G2jnTLIm5pPULDOBeiXV9twdT1Uy3%2Fgyjj9yv4K%2B3Vhdgm2%2BWoJXWrploFFvF2GoK8Q1KUqAi0t0hU5klMSbKJbEZ%2BYBGi69%2B0bv2wnjjHcmNcV0hgOIdc3M3GliqDvysq6P56Tt692jnpN9M0bZt5OkHWSAdC2G8g5t7DhFnpOZPCPdwefCYGWt0qOknA6mzYOGxqNJ%2Fphaeq7o9z1MCOx4wpnNGamd8hichGij%2BDuyCrJIZWz3p2iLH38%2BdW4IYO4n%2FVj4E9Ldeo4P8yox8Cqo8T2ndf6bGtCYqfiC16Orm11z3z8yPYXwZZxn3yTJgSO1nC4LG9lxBjcHSebn%2FcV8QfYGCRISlsA%2F2B2nogdg0dsKZmUdBk281jgUPAr56tv6RSjDt3a%2FGBjqnAd4t%2BQndTwFLz3DQ3c%2FX6GHRHp49G2yzV26AictYbLghcNhh%2FQ27Cwl0KlpwNMMsgJkYwGdKGzHRr6UUqRtQomZ86Q3w4U8wjD8uLTUiNgtYgqZ206Gq99FHAfnfRv2Z5f%2FyKpvGTEp9gIfP0rQt1UAYL42JWyt6eorauywo1Nm4%2BhbnyfDPMwq8%2FDLVHxtInxOrcOrqprU2cLMesWYuwuD6yMyLoG3A&X-Amz-Signature=fe870e80b51b41126108339a7359f189551dfc12b2b2b8c636e0f81d13069a1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

