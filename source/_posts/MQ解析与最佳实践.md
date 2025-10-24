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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGAJZS7Y%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDBVvG9XqKG%2BDEa2WkWc4zmMPLY9XmrpjtWmXz%2FkSMLGAiAjhiImvdDWFaXiTT%2Bt%2FnydH5yGsGegT0ODAE1n6hyMtyr%2FAwhUEAAaDDYzNzQyMzE4MzgwNSIMgjriIbrWegbjGptSKtwDyG0TE1cXImFPFVgRNVwgwz2gvHVqObcGXIccjIv0T97msYdm18m2WTTC3hiIWCeJZHAv%2FsFoaS3KWwng%2BRDSd5DR82%2FfiLZiCZ8D%2B%2BrxDSFUuHlv9QW8OmwHvN%2BuEOSMDpeJJxIxnQf3HZej%2FilUhH%2F76jxy22XL4cX28tI91CNM5NeJJJjaCqlcRLaPkPLq7dmwYDBGOOqigaLsxyu%2BbtbOox9u4vE9Z0SdLEpkorCNGC3M2BxXtDK%2FWvix1cIeIF5vhfMdiJHdWqBosD9uJSC3%2B58G3YraLJxYjbXSj8Laq1HTnfkdCtwpq6JElrnS5864KCkEFWre9o%2FcxfWTNKYMFZz6EXpeYySciFqhaEeDmLP%2FsiNyq74KbJfS5RQq09VRyB8TgyxV9tm7Jd9TpHjlZeyMjE1XsTU2TyrrAhljNVDMmyszeLWL1NQo52E6UkFECsZabA84Bk3we6CQaJHeacNZgXVD2Clx0F3NOhArJ%2B7Ae0635MXyuDdqfhdXSajiaqDZHeJL0Zbdv3DAotyiHeA5dzyGJJfn%2BL5xnCbaZEageiiL7OVVjyOQzdpXESzuL72Fm0KgyxxkGeeIFUgg8I7MlRyPjLIpgI%2BBUSr3PIUF2MJRFf7qEzMwm8vrxwY6pgFvzHPMdKsG606bsgd09efuEuIIXHVZ5XoVaJwADZnN2aykC0bq7EEpKRQNhagdx3fJRP1bSd5VL3IaClSvW55yuAIEc2RxcDiYWZFvb2H5nuAm0fRcDSh%2FJHrqT4oMWNCmV0FEsPzh5pGKYEQmsk0bWz8g45GGFX1zTg2kzlPjN3gUNmqQpPSNhEQd5X0x0LqS2UXlwBsEUghnnlBHlNCYVI2hSELY&X-Amz-Signature=3d85f7fcd9c00aac28e6111e53f5043b2abf04e091b3044296281c72afaf4890&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

