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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665K7ZX33L%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJGMEQCIGA6XeLhuoMc3W0c4dXWUemqndsxlnvhZTb9XBYJTb1AAiABIvkZli4KPhOFYYmjkAEfeNR9VbxQLG8jAb%2FopuqbjiqIBAjo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6nUxSTL%2BDS3vPA6mKtwDK7pYToGfYyLLfqZSG8z9gt4p2LFp41HyH7yk2e8UCsOBawgOiUv45ECwQjgmY%2FeB7FoyPmcS4nKwWC%2Fo95nYBQ81topsZFjDTrMmVvXHAjGgEnDooVGL%2FxJDtXaAwsyTGGW68JnNVbLb%2BMiIxb90fkuPMhgdImk6Edou%2BU58Wo7IeKxv7mPXiZ3AxLagJlPiUEQwq49c3fEVTsFH3XM%2B5ZOZt%2B7cOClwr8JxZwEgkiwM94IgJbNFQ0SrMtI0ASFRU1hbqtcO9TtXA77M9gj2o%2FumKSVXorLfcueqd9cDVeRSE1qOSz4LvKP1Z5Nto4N8Zmna3IcQD6ZlweRRxgGeEo5PhEM0h5T0SK7o4ewxNTlqVPJp24wYnHaKkFFdgiUA4dOUsKob%2B1WI54pdjIHVwMY3JWHuChX2XxfC2iDPmcU0cvxDc6vJulGtE6LM8uRvXtahxjsZFd8SCvO5xqwiutZs79TReEp3GuDCAmr%2B5OwWnnWWPWXJMW5ubzcVC3%2BGjgSHOAlpeG6C55z8Cyzv1B4xxLxJnmmVGy5oDZGAlMoEQaGNrUqQ9NcZSSZV9klYJoHN6vaRu5jSJmmLaf5iRKbjyiMCH2ECsibTwNL%2FE5HB4Zj3p3jbuEnjt8kwkJn5yAY6pgEHm0O7rcbwAmWbPDDSINojgixDyr%2FWVxMsrbZOCNoSN6y9ONEYmewRPB1jUfBiPGpMCVf2uF1UdXsd2A45Xw8sA1Z6CsRNznEQ6S3oLXr9%2FUhT0xwb%2BM2YJL4LX465KAN5QPp%2BUi6%2B76l%2FaLqJo9T9PpCBXof7iaYLZ2iQknQG7OuHZplhQ6n0UFvbB%2BaHpYGn630lz%2Bhvlc8s4VhEBNDbOMgDIghn&X-Amz-Signature=092ddbe323c4c65a7f316f40e975bf95f476132df658d13f2bc9511f058b9535&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

