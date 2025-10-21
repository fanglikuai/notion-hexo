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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QR22CFBI%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T150140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJIMEYCIQDvcs2APVE47m%2FJpzivmLFwT264MKVOndWEw3R4BwpacQIhAM4BsB3J571dUp2pMmci8Rhyy9fJhaHFxcwK45aa46bwKv8DCBcQABoMNjM3NDIzMTgzODA1IgzStj0ZgeoGqbuQeC4q3AO3gJNLFmjqtkduI%2Fi7SQEnYa%2Faeid91m8AG2KVsKjCdghXif7y%2BhstAmdepNPM9FIqQaZ4DmCfCeoG1abvWBuAznxWr1jsmXhVyKuXs4bz8GZCta5cCiQL1dsab0Iy4Y6Uft%2BrVQwWpqW3Adpb7jVucIng5kye0IF%2Fj33%2FwNz8xvTIEgERb59W8L9fRjLqvOA%2BNFtFcdL7rXYZwyWoe6b8ha89qX0XkSIXDlwxw4E9SH9tDGddK%2BNttLQwHKcxPeZ68R6%2BRWbBTmvu49X3ViN469E7%2Bnd9R9NCxpdU5LYlY6Qm4Eqd9Qc42OCGeuvKrt5pKDGTCaQ9DhZIYLdK0YkKSyl%2F5DjmIrO9aDohMBVA0l3WNJTYpQLzOY4PT%2BkjhoUOCJ7jzsq6WAOFIiBgKAqD3MznIYtmBBdXfXMpr3h%2BjikKtxLXPCqMQPH0DFWnYJDsCd%2BeCxjPXwGbst1Lcjrjf3psMnwkZokqaQ9xr%2BNALSClsqj0jUbeABOJvdQt9Gdoz4HRmRE%2BPMCmrh3y%2FU2rmAErG6deXUMKSW4OUNrwbh9Xyir7UAQKCdzx5mGMGdJvB6DtE6sfSk14IC5uf5efYzm%2B7i%2F2Uz6MpefBr0wOMEsBDYOy%2FHShMZkzhTCprt7HBjqkASL8KRW1l9%2FdLPj8C8MgEokV2JVGwJ%2FHDPsoXqK3PMJAfQUhgwfbKz1tnQEoRBh1HI9hY0HhzFLLj90ZiWHNswvAGx0z4Pm8Ez3rC5UZrtKvZ1GB6HUHIN5EFS3Bdd13q%2Bqp9JNNRXcbk2dPuF7roP3E3XjETLeT1GaIl8%2FPKiUhYbjCGnZ5EHI%2BR%2Fp%2Bq0AonGJNUKb6UyFmmp6jKLC%2BDJBRJG2P&X-Amz-Signature=e708e49c215af258f8b2dc9aee8c90cc79aae8e2a3c92b340618a4cde5f94e0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

