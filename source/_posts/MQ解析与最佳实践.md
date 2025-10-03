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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IT2KLBA%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEKf15HoMDxjdOtqjmyld%2Bd%2Bw8xFc7v5d5o3l60QPbR0AiEA74n%2BbAGMkpHUBFbTM5KvFTdaP92xWsVaF0OedXPHiwsq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDCiKoyX0e0rjP44C6SrcAwTR0LFVCpCWVbzsRKuNks2%2BsjkNYoz2t58pWnA6dQxgpnYfweWBVUVB5IjqWmmJ9TZ7D1TWuDzWTrYD4GS2AIUbcrYWINeAziIRUMoaeBchJCGfUbGeSJQo8qfn26LLWZW1FdhYJTUUjo0l9Gg1tZ1b0xpi3fF3AkQf62qJb84cvdqkFY86z6MXaDmGf7tCxUkaAJJ4Henb72O1IUJK78P6W4gjLhKxUy4AIndiPPB780QtZJ4MRQGYxk2SfX0rg9mctNHWCyM9jHTE9U%2BOS4lrnU2aJrmEHEM6LET4FVbNdHT8TXghyMxrh6%2BY0hj3xFmbOWS8wj%2Bisl2RR4Axi%2ByfHAPJS5mUKlo8vPjKA4qoIJE%2F5mN7vPirf0V0nEEUy9dujgF%2BBrAsaMpandBP4EJB9XIY93Zv89a2ig7RChDfXalwTxEfaufJA4j1Bn1k41QXQKwvrDPeixMC9ZDMLGfUHle76ulv29KJuXRnaYfuiEhA46IxUWVC9YMt9r08d3V2OcGTBX%2FpbLyUAxpXtyEzqrDEmUDIMvM53gPGUiW9U1VXFgF6f7nvx8fyM%2FtHH8GlQxHRzT0oNdZHVrmH3FnFbBwvCpJuJpXZRk%2BxoCqpiRAuiDOe5sxW4KHEMODS%2F8YGOqUBkdGvixIrd7%2BUkiQyIlJa8pEhJ857U2KNIg9kbgvl7YoEAtPiXs2W829bg2at3oVluuACn%2FFZMSjomnawYxnbSGZ%2FzKzrzp4R1WVHC9kOM5vV6fyru7sGkyaOd103MQHcg3Y0II8R6gtTTKaUiKzgrYev4KLe6IlTTcTobPlqnGjlkGa6UqqjfcAPLIF%2BjMGphnzBwYmFypQDqC0ZBCrHruddNBZj&X-Amz-Signature=568d0e0d247f11bc53372e949e93e13765f843d5d452c5f3d3422e988e7f090f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

