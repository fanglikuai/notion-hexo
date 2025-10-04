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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSWQ5O56%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDsmHmYDsWfszG9FaaA1IC1Nt0dXZ%2FhnPWqSoMNhXDaTwIhAI6ednnCFXWwQeuXW10lqWslg%2BtpzvMlnZQ5aWc3xxFJKv8DCGEQABoMNjM3NDIzMTgzODA1IgzPJd3gc4WvGcYA43Eq3AMeJypH7yWLinYDIOhhOgHERHwxnzTY%2F2xRdyvMaS0Zmrg52g6b4SunQDCHLdnm7N6tRcaxaWQkcA5aQdTPwM8U91OzvIH13MEJNjb%2F3ubDTE7yyztyWwSUkBm3%2BvibBJV%2BlRPS%2Bh5thXScuP%2BzsH0esOX3VcvSS6utHRTmDVjkva8WuTHjbCYGUEKOa8pBdd09xGUpmUp6i1cRJMPXqTl3%2FY4qXzikyapwCQKQ5Of9pAQnSU5tZi5zcWVEyIciUxr0kX4mFUdhAaYa%2Bh52f7fZtCMAK1yik1HCqykw5MgpiN8kI02CNnDeU2VVbtcpQYy6sUl8%2BLBPb5NqJqKWFL6955%2Bd6SxaltxgYeeEwWRuxagi%2FY%2BC0MSRBGayLo%2BOeoUXJPZ0hKU8O8SZ7B2vte24roY9vgTkKnApK%2BkCvJsj4QhEEF729qti6NZbpS5l6X1yCNnR0KRZdnMwI5VhfuGoYsqAmWO2ZWVDPsysn8InrY4H6jhsqjMY78EDoLmBUrkGOc4XHWBolN4UNk2mTytT4xgFQf%2Bj2JT4qrGAD%2BE2J5tbxUgDj0ohnFp74GprNOPgwjaBbfDLF25yRndXmMMRGMk70BUpOQxzmqltjV99BZhJiT78mQLvnvcycTC4kIXHBjqkASPa0URqAJv0Xgy%2FjOOzBA4VFFh%2BIbf3tpplv4zAjsH82Kl0%2FsBElm1caN87IEtb60FLxC5F%2FKrOCvygUlL%2FoCsH9DLJF2xlRs%2BOeF26OuDoGgyXDdoFfxJGbgNhUSnvqx44lKNH56ER82YNGhQ5UdRIwe1O%2BIt438FnIq34oeFdmBKmje6%2BTHk%2Fn9JxXm%2FWuul1X%2BH4b29ByHcy%2BbxeKEUGogsA&X-Amz-Signature=5cc87623e9a26a16457abafd506876de5def97e06f9235b2d5f999f889b2070b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

