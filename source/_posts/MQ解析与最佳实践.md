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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFHGNZUI%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJIMEYCIQCgkKUHNjFxZesaljgk2KH0lIThdbQVteyvuZRrA3u69wIhAIxm3SV5I9UK7h20aTM7RYSqCKC%2BbXfQ76RH4MnysH74KogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz1ybjLXMLnOyarysIq3APNyKh9ET7jYD2lmFy6HG4PqlVZVn59hdj6C9fAWgQ%2FXZMvHWW2De7tIRAEKtdme%2Bb%2FIojeOg6xhD5jbqbHCNLOC3MCX9RseLW%2FTgbV8Rgu9Rw%2B5EyQw28eeb%2FlY%2B8zyvg%2FquJl9PsI9kFVJs9GdOvcprWUjrFhRRlWd%2FIC6wGHuvqietJFFx6DFoztRJQ5BCBP7Og434E1pTBI8XHvZHuBAgQE5UOza5YmGT1abrfzxFVs%2FptxmfQ8g%2FqkhlnPLrIWQ9YrQuZfVkqIHT%2FZhXyh6n32h0yLqDvHlqPrvPusASTSk60BRY1a%2FcPcY8V6IHVfubPf75PXWzeOmlVoTBra4vZm5BIHY170%2Bg5D2QL7%2BPoa2SEGe%2FCuaM8jsSlEx4zj9s8nmxqGR5j7TMsJxmT510tBLmMoEfpuaHXl3chn93syq%2FllfbiIOySuMVLzo5jSM%2Bugjwd%2FyuM2Y2jYbG3G0Fjzhmg70c1hzx3rz99J0njKyvRW2I1aLDARzcOxlKx%2BcdQgJQ997AYkR1ANxZaYFRZkBNK6%2FGgwV%2F%2ForGBg%2FL3LHkiCZtfqdntn%2BjCl%2FMwTXQiNEwZjZgAEM412RGR3XbIdQenEyJWb2P1%2B2bjqodPg2KaWDU5XpgqjJjDQ7tDHBjqkAQaBacW2AehgCA1pWmVM5qkDVWYvr5u0ydvc7rEpjBoYPshM1FPKHasGv6eGdxpiMeiJ4kfoSgI0FVoqC6sN%2BOR0W7VMqoMy6gId7DSHG3E6cplBr%2BjCNPBxmAzOysbLTptPkKY897aZMF6lvv2iIM54vnu06iGGnDit9933Q1XV6agFLN0LEpXrBcyUxwLU%2B1ovbk6vUyXI0jbosJEtV1yT7gMm&X-Amz-Signature=9422d009c5969eb165a2ce0e32771d9a2d8958c19bed5b84bd9f4fc2977e2fbe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

