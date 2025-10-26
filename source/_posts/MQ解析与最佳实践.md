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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GTQFXXE%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDz7RkcmxI1zbhFiIKuvmCiBBPFLm%2Fd%2Bv0HPVMx3iZw1QIhAOq6xwo7GtHp7VyQVzr4vNbv2G33VXQ761wcsuVsWDVhKogECJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxq3O1MYdwYOgjHMYQq3AOvBk2ghuSuoiaDT1CqKHsKPTaWUwvZrqeJ4BJECFeOw3crouBSIGfkMIs3a7v8vzdvsW6GO6CumUbJWzLO3NCDKYY5T2BxmsjsvlMECNfnOqNJl4v%2BDFeHF7uV8NPEXuFfSm1Q5gsiMUOadIlS8eE4T7wrtDVedIc%2F0fXDsowXMDsi2UM0DrWCPPI9M9wP8Htl22VBnJQMuyAVjMhY7WKdKEzhF0pOUizXivu5tC%2BgESdVtHuQ1EFuU7qUuJgVjtaswwL2tHFQtpmjdfIwrCTcsiyi%2FNFu8iRuShRgSUwabj9dw5sNfYyi3ezmVChAoqWLNoS8tsE4G9eU3VS1sRaGZf5wpLS8o7PDsdkphrmXSoNj8WJOBn9qKnFzYEkmDlzSRjPCUAapAAjk28BDBWijq1zGb06Ib6O35XFV2o70x2VH7lMKmegN3N7yKOyyXptvKSNc%2FlFkaVG%2B72mFm5TIabP4HrvfeW81PPlITELHpqmD1LNJ3J3iZg2YQhomB1RT7Xnfga594d1DlLWLnQxb3kHvv0WLUPRVxsf%2FUb4xgBmaQjupAg6rI95u%2FLKg9Iln1X6dD1pjNOizSisB7IySrfeWSYy2sb%2FtfxhdYr7uUQgociUcnigoA7%2BcJDCxkfrHBjqkAb69GFkDdCj0thxvyaABol8LPO5SwO4s3TD%2B3IxcNduj7mSPsazKLUUCEEoBB7YTovkjg%2FR9AJDF12T%2Bp1B64rdm4aCmLpFkF%2FZYO6SSyXWiKHYnsMZHBK4Y0OCRr%2F15L5bQJ9gLFeKMl4rckdwKkEGC7jpmNbWij5e5jioKUtrBTtLmkypc%2FrLc8AhwpZk8wbaqDQQkiOYfy3gQ5jCBxHUssans&X-Amz-Signature=c1f3ef8e7268b674d813e4129beb5f6e5a46d3e62c0da75ae11e38076bdd8126&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

