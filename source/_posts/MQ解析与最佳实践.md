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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KBIJHCN%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T130038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDsN2%2BBbxvsNyw1WJ7fec2E%2FYBpAr06Ozi0mroUz0DOjAIhAP7Cj%2B0WsK0glttbyoyffetJKbEqfdIZAerBYUNIHXzYKv8DCF0QABoMNjM3NDIzMTgzODA1IgwyxCuEUjEtt4cTplgq3APm9tr4sGSLz2Nand9PSg%2FxUCr9okr8T1mHXgMd2Fc63xDtpKaXpyaIZoitugwHyGOvdWiApduYwfVpYU9ZEP2TnWzsR90WFPaZRxVGNngmrntRni5rhWEKP0%2Bh%2FI5vlcn88fTnanrEuCK44EQLMupdOPRJVPlgiGQ%2Bv60GZSkv%2FfbI3CLWrTmZNS%2BsilOXZ2ZPP7qnZ3fHNo1Ohpl4g%2F%2FkxpmlW%2BPgDwggrclOibuQfnwvGx5xmiay9lm1e3GaYDTW%2BgiuvIDTisAvcjEBv0bOM3bneAwq8bbqPit1nB%2F5%2BAAEiCXt%2F%2F6aLNaR7ys2yd2lGVWQ0fLWgtKKI1SV7YlnDsAi1RCN%2BiCy7QU43kAuBDhZ0yMTUmyDaU8cJqUJLeILFkxLwY9XASwgQk42SnYVc31CmegBMyeM%2BbCztyzJtjbeAmbyKkZoHC0sVQitUodoLOEUqWcLn%2FPb2n2sjjbBBQ8etvTfoQ8H3oLZCDH9VRxlJ9cc53j6yE4YLrETaBdQV78x4EzD8Qa115f3z%2BpP9FaOeZMqzw4tZ2EfuqfcchRhRIaGfqBgHQkG8wMvvwIs76ajXxqZC6Jmlmwot%2BryojkvX%2FzTfPcCsF7dxrUQm97lh0yopMxOGqpIgzDm87jHBjqkAQVqFFkut60GBlFCnE5ORGe%2FT8a12mc8vpcg%2Bqi6qiuT66IMu8xeUE6qvnb%2Bbz6AcS1VNHEAFbkYpmgSZDUHLAwBEY1%2BV6A9bombFpmiN3ZVLEAGXM176l98VFEgWBpUg1APH04qoSayDI1mbhW6jNZy%2Fk8UgVTVfz0UuEhmWQwYPUSuptKBN6r6xOBRisWkgSD7aVtPDBBYpKSmHYkP7BXnvryH&X-Amz-Signature=3be43fda2aadc142b0a4390919b4ddf4731e25c3b1d4977f5bc6bb9f7b8d0cc9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

