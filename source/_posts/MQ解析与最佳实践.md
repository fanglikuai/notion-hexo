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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNXRXY5Y%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB8kAI%2BtwrPXCAhCxXgzg0lkGHY4tO2NUtfxX571EvdqAiBaP%2Fi5z0EevPDW%2FjsSKGmSxhSHOgiQ5lwRhZEA4Wtzpyr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMe6WBOYwRdBHYxp7yKtwDW1yLBg%2BNo4AD3oOVy8WH2BVlrvw08FKgIsS64VMxuWZJtXnCSAuLCBnMiZ03F5UNHijn81w51OZJyMosI5re0p3jNK5ayqCrUGjmFP7%2FqXnmfqHhWmCTGsHBOxnPg%2BwFCDspb8TxkHuFzI2bwGOHpPCWd5wwgBCp4TTAmEVFZ3TxIUFRZxFgV5FnDZiJgq%2B%2BbQUC0zz51iBLz%2BQ2WBVcBwHjtIf63v5POROzWP5mVuyJuBdaH5E4JeLd97xMErk1Slz0XVt08tfpDK9szKcVOMm86ysrRBfD0ly84EC7PHhbY3VbHkk%2Bwqr%2FCg16a1DIb1jLVZZ2g%2FpLIyAryl5tn3nt%2FknEpfvOndG9tiADRS8i%2Fy72dZtSzoHJYiozH77uXoSc3EWrtmFpqq6OR70kcVEti26QFweEcbeGWfQlGCpgKR0xkYlzURon3YTr%2Bi1EcUvMr5UdppWEcbw3oHYnLfw8oT%2BeTsEypjI3358ySbKgEDVXbnXXC%2Bt6j9j243MMcJ2vLSgjCJeOzQHE3hE41o%2FdM%2FiPez3dODXl8r6Pfcm1sLYK8TKwLnyTLmMZRXSHcizVR1NRdaMQQai38UXM9LyLrTVGzsL994eM944JXsV5%2FSFnMjkRrIuUiO4wzqTWyAY6pgFuUn%2FPlfnXNqW7tCDWSYOrd4gSPwvIt%2BWmk7nRwrmAw7yPx5H5EHuLMAWGsq8BPv5fndhNAZeFNw9mBEoZXq6rcAVAJQv%2FUpRkkP926mtUuA4lL13dFBWAYjHRP8ixaWR0OiwREFkTHlWyPXEK4lL54UxlW%2F7Z9TWWtnCVwF%2FJeimN5siELhsE8ysOD598sa25UKzlBfxapYmE21gRO3ovd5Oox10N&X-Amz-Signature=704433348ce6064a058fbaf85447b4f7f517b252a917b1afbc1c46b420f7ab2d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

