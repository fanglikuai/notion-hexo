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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W7HWVKXN%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T160037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJIMEYCIQCnRTSqvHg7aJqxFTGD%2BlFhJAlPsGKuIhlU6W%2BwYZziQgIhAOndnz3kEcyugRk%2ByrCh27jFmNVQCV05lhjXNxpru%2BubKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzJ4BC4GqPEVgDpzH4q3APM41ajo8IyQpwi5m1IhnbAbfBgG6GfTq%2F7l15g8sJz6HpOFzimF9F5ABr75vFztF1DDzjQ1Fxd%2Fhkh11nsPOeDnljo6Y3%2F%2BsgJu4HZgwoRiYXgCxb9XP4%2ByCosOp3slyLTaAoH8lpWMJ4jsHTLmS%2F8PomJ0fQaJZTJ9aUiYKWK63HXRlhEv4G5TCdRhNLjB4%2BDJihCIBmrqIrlIgae5SB%2FzdJupUyD%2BgpiBzkdt696vf3SyXTJTYsraKIMrO94UdplI50fJzBRP5EagALBpKpnxxrFt%2B%2BabduJBUIesffJdqBNuPTET%2FZZ0yfeNbz6lGa3VeRTnafKowg5tLh2ifE%2BLSnOFTO1x4Pb6o1Ltx6Ur3u1lE14IpEua9H4vsfLT1tBCnYAHiHfoytb0%2BwSTDMHEw9ZUXGrG8Rr24h0SixpXNMSF5txirlqgAQ80ZwABcrx9KaR6nCKqCMUORh89xjoGJkpYup6Il8dTxyO4zUBNLWNUJao99rKkCctP1hk4difAA9j2D1tVHSvznfpLoaI2IyJkqnKZHQ4D2kgigjG5UsFCbT2uJGF28r6MBNmykkqN13iwt2XzIjEFxnB0ObEEVdpdQJ%2FGQ1emtk%2F6jarTq9lGpLaEj2Enj%2FWNTCXndPHBjqkAe06u6L9MlQD7fXVkMI3DBl3rr3dWQCpq64m4z2Q0ber7MWrISQDwcXeMBJTa6URcYGlKZBDMuTWPrP5I2BtQrLK5lVTWNMLwUOPu%2BEwxAdl5s4G049sU5LckyJd09fa1d23ROeOAVwbRybx8bTrW%2FLnddeJpDg2ER3TsxZQcqJkV57avSz5Ipjj%2FTLYBlsayi%2FzmgrDpHgeslk8BbB2SMlmJJRp&X-Amz-Signature=ceb11d126fc41beb80c72a6c69ae5a6fe5a45fc3d7660de2c2d2488eeeebfd7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

