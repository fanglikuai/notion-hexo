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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VI2HNJB%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQC4TI0uKMo1JwoGI%2F738x1kw52BihDnR8jGWyotzAXmwAIhAMgn8WuniM7AlBBI5zOLa05ailwtwpzd72QKqSlZpVK4Kv8DCEMQABoMNjM3NDIzMTgzODA1Igyu8aIEm1XrKLUCu64q3AM9KdOrtp5AzfXMsh3XdYPKnB6fuR5HFp93QNoWkBJe3ffdln3tb4HV0IYpz5e8l%2FZD5mIzJG4kWp9ADYoXqp6XM5hjS8LRP3zBZOUL9V6C5DV%2FvvxnShLzitgT%2B3WzQgJBJ%2BP67oXuWn%2BOD9Z%2Bwl2fX9iispTpB7Z%2FygI0l%2FhnwxNzhW6VXlGrxRFpkYyFSWoPKm20EHJTj1EKpyIyCw8dNR5CpScDTU3tskiBmBkHIItIgloxoSOTF3vrsqXQh1NYOw82HqCT9TQvqq5UaXAeA4D12%2F7s33fWA%2BtOc0vK9F9fkHOxJUvXS%2FGPwlk9PoyPe8cr5egF1Yf8URfXpOD%2FsHAz2L6UHt7UXHiPo%2FWpdCBVcGtGgNYsmGDjMvJk4J%2BArAiyHK4r3hqsjS3BPA8zl3%2FZXwmFL0iewFdfAZnDH%2BbQtAI365kzmcm3dVuXO6R12a%2FNEmiRC%2BvbjsuvAFjrV1mrHVQqnZvWR%2BpqRlCz94vNwof%2BndBsNQNDK7sz9ku8tLKzLoJMXKMSCebnxa0goTCMqw77hVYxnnEKgJTd8%2Fg%2BkjkJg10Z2gghEnajYeFPmGxOae2kp6bxsGraI1P1EnaSIUxdv2RUgZPkinvnLwepPuUcpesMAR422zCJ%2FNTIBjqkAXYPy1cpvN964iznqCJiJtOjzNs4l6d5UhJjuVtNm4Pj2ki3PmcWxrERSIY9JEombeSp1Yv3bp9CSx6Vm79SkGtBK0IhLMxRPNZCJA4WUtdGYZkH6y1FdtvuX3GwlSF9k3kNPjWnz7OkpmCOTUt%2BEsX7teMbbz40Ve%2BX72475DivB92UeZ%2B2dKHPDp0g5KFykAWgM49ALkMyRqAbynyjDf1loLnz&X-Amz-Signature=b250d48043e68175cf36687ce3cb5f9c7cc651d2c0f2a5fd157172126a52d89e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

