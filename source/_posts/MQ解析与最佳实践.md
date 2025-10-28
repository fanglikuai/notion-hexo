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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W5VXZDQR%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCfpsiUA5ZAj3gcbo2lO9cyQVfT0BUimo4nRl1TkAH4rAIgFc5QOKACv5aQDLbDVE22FVtlK7AcXUjI0Dtal91%2FKuwqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCVGf8Z78q2OUTHzmCrcAzm9IMkoJY%2BkstwnW9muJ%2Bia8gpnRCVBzdGEsHtjoLVXSU0HunF2PUK5Ex8kEDAzwq5xnib7f2iMsw5doQkTstl%2BpueCjJMzGreVWJGEGRDBjSO5sn6WA5yVLFGj1SWNsPOajFoX1sHeOrvzEmMm9AXMQZpeTCt9tJfW78A8stowJMk2PS3f9Nx8qXDG0SXbm7y0uge0kMCbBOglyxXIeWxVGDe71tUV5UfCF44paWLS9Ia9kEjgdT4%2FfKCdrJeKkZb8lBWtlin2v9Wxjn%2BTGiouHyO1VQTbPWaimY%2Frd2DJ6sokwuTrWEtcMD%2FPdVusL2V97V2BFJDVNbhqRwGpsPPVo0rkW8U%2BPFLTVB%2FZ5ErfRLvc3oSatM0PGk3gstpHU8pwod9qXdTIYmc3nnkUoHD5UFzcS2iEYIyjiGTqC%2FA95CtBlmXhW75Wk4YG52m%2FzvrU8UgBXWsVU7LxEkDJA2AmJUeLyE%2Fk1x3nROQ2gpWLrNn1HgtzQAvKzVuYwJg2AGcD%2B9CqNpXq8d9W%2FnuS0TVG%2B2p3diP8BZrNz3pxaOQl8rhnLGpTTbdvehzqqZcd%2B3vSfDHLLFzcdS8qhpeAY3nRIi4t4y61A%2Ba2VpVK2QZTemXkKm7qV6vWIjnWMI66hMgGOqUBPLkS7aiJGp7rRGzljnVXzJCk%2BBpgZRZ%2Fmay2EyJP3i9QCg5Ej%2BFL8GEIVGJu3db3sjRn2%2FSGa7nOhCTIR02JMbd2HoYcu54l%2BG5cN%2Bn0ATAhS0l5xRccxToBGEvox6y2YI3%2BojHEUt76kuLGxyU24cJK%2BnlB0AcD4Pqs8sRlOFIR5TnhaGGQZegewJUU6itIujsYLQXZV8JYqZFakxcaNnwPWBIe&X-Amz-Signature=bd8a92db9b73b319bdf08b9dfc8067d25b163d4be09f9085c434e9446efe6538&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

