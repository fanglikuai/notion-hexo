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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7F3O6ZN%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIEuBGhWa2i61YpMYmHTQLv6PGnUSH6BLXZYPNGOsWfudAiEA38XgOPkd8N36Oe8Bmkzyl38a0P3oXJmAlykmjHCtqqMqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCl86LnEFxYGE2bj0CrcA%2Fp6BysZ3L%2FDMOqB7jJ%2F8U2pcrvA2UeYOlgeftiKtAUmCMgwiW4bIsrU%2BYOMZ9QFI3skhqE%2BIK0ZXQmJu5l%2Fgx%2Bhib4Nlp%2FiQnH9D1DRa0bLJ2SFOt0MjhxyTeWc6v6%2BXWiNpxOAWRVoGMF6VZj0N%2Bh8XVMPzIx%2FzN55GeZnBbMp3gPzYBQDFR7CT7Oa%2FQ745Q1Z%2BWUaz3WON4vKHtMOPElD9LZO93uj9MQpsn%2FUOHbOTgsyoudyXyLWt53I8byYOf%2Frpphudnb6YSjtom9V7WVoi22AVeVCvO172mRRYBosZQwNGoSWYOuu2%2BpA390DrCFgkI0rliKWe32fhTjzyUQALL%2FpbgMF6nDG%2FL1sweCNMNlFqFiEvxzVKsUC2eX5Xcwj2P47Qafi3a7IpzyrnR%2FZmIM75hklIvQykOOgsx3%2FSywojYEUxarVqdV215ImIDaRDb37lBflYUeUV6nutQ8oPPOlxeb%2BaVWP%2FRo5cAwkaOVgSyHf9H8a3dqR3%2FgkovQsTD4t8FFI8u%2Fc99BLQk%2FPrEHko30fXEKk2i2sw8Rkt70KUqSQ%2BJvvLiS503TUIlLvpWxzbH%2FZxMq3xTKwS%2BsIuAZxYjVpxZqs6mdy1osqzdHL3a2A0EVPzb%2BYMLn11ccGOqUBOzQfyqrTlAbI9Au6uZpf18neq6xBDIVG8JMZOwNMY1HJsbgroozMbsGGEOMOliFvE8MG9EzEpY0o%2Fz62HSY2NDUlos%2F%2F7jpq2%2F5lVNVbnJ7V7nO7e%2BfjQNZ5vUdhWqprQtvPah0CUUu0lfJYbQSslG%2FsLHYPmhwMTkKQxm1Omk1G2Do5IDumeTlNh509sDPQbrG76FOz9TlZqshiseTzXTvPHN9Y&X-Amz-Signature=246a96f77109a37bdd816b3dd2cdcce61b6b2eb0c5411ea61ba57c529b501122&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

