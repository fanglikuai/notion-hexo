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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6VIRAW7%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDKtpui51roPUQj0TkQ4aSwWr%2FixxaBKZbanClsvPwbsAiEAxAOPdMpWlVbgAg27ZHMPqMlJAtPPfNL5CfqaDEH4Dicq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDAopsQTTnXcVQiFjzSrcA9JyVmeSoelEYL2OUxtHTRFutnlJ%2FO4785dI3TYghYGD7iI0cJHEmIn%2BW5PUXBgFHI8DuuT12NYyyhBR7QYJpNpR3QGAbQG6LU5k1AvTevU43epO2dpaq42kfdLGe3xKT%2BoZi25CIOtFiB0vWKPk98f0abp8uunViEK%2BF65n6UnEeoXmOlP0p8mnHICbWggCQzLDAY25CTPEURs5EcUwHn%2B4bnJa9EWKHexV3r04PwamZAkWVH4gl%2BDWaxWz%2Fpzf4DOv1OOrgyLcTd1%2B81hEe6GKvvNs27om%2FccEvi9WwIJRO0D9mRL30Zr4V3BvAV4x0OigPSZKBHtMKA9ESRdrTWTDcb19ZU8WLra%2F5Z2Xd5FO4Vr7IiiqwpAFEDAtDs7uKfxGFmbsz%2BAwcgp%2FUK0CrMcHecbpULHB%2F8Nj7StLcvcuzN3zf6SZHjN6UP0Hu3FPjdbG4Jwj%2BYbgqNRRu1KK7mqYNN7ne7wDTN%2FfxfAApT9OcmKFWv%2FFsKhTfwGa%2FV86uZn1XtlqfS1Az7Z3WhaMdOXTcfcHpJlbQOCTEPMLuxgT2lVp4ZhM0hlJaxBY6%2BRJSa4XYXIMhBeQjQCPlpWZJ2BxgCXznbfaQ71gm4PHLJ8X1WOJJfK6JYjEp6pkMMj0n8gGOqUB4HiQJzve1Jljelu5aitP5K44bYp8aitc6NL90ZVxrDWKqFbpW1IH6IjGPNVT2PfEL5LTwB8f4OWJ%2FP7iQ4kntj%2BbfDHI20%2Bnt0rS4EOn58YVyXdsPtxBjJtNCwPmBRTieM9WX%2ByBhoMjqQseoxUfqjYyhYFW%2Fi%2FMwqkpYZvJoQyWUDE7tEzL3RFipWHXuOCtdM9R8RCgNRXXeh2%2Badf%2FsLtSfcW%2F&X-Amz-Signature=f6c2830601a8ec325dcc87107625c260743dc4a3e40a1ce5d7eccf1da0147c9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

