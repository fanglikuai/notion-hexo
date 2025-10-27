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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7CKZCJU%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3HB3JJgpWQCAwShfAFJ885540iUovHJljdCmlkq%2FgBQIgHBozo7mqKayytT0J7Ab63UBnFIBOiC03For8qMv0WigqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKdQ70ncmN1ZUet24ircA9o4b%2BL09C2FnGpVOVuMm8x5QxQfJjMz5I%2Bk4uUKed8XIGR%2Fsj%2FTO6jg%2BKn2Rtqnr06%2FFhYLcqBEXD4tFYsuJTJMIVeiXDaBEFsJyWoLXSe2alSVCpMoiMlGT%2FC%2FBYc7%2FQ4R33OutA%2Fzl53CemPkVSVrzJnVK%2FYZYmBKsNG85C1d4Pdpg%2F89VFoi60IZw81MbBer8ROqXXELRhNR4IP93%2F1HlC08m3HEY88V28keajcvxKAIMqYWb8ckET5TT2ukoxHKHZoVpDbDPd7VFhNxS6TcykAmcqFknFPV6PE%2B7JoPHV%2FMGtqFGeXHqP6LpoI7qXSVj0i%2FUqlr7yVY8sLsHYeYCxlh9PjwU3o%2FeU3pU1%2FuESJYWOVE04wwRtdccjhnvTimnDAxO5a%2FsQKAutbsBLlA%2FKHKdRVQmKLoe9GMcWs9DJq3dnbAKqOM%2BxMfN6YNT4iX16YyEplerQLtVe68GcOvSaF8Ii%2FsSdQtBn4FHWOJP%2FQSidtaBe3wg9ykotTd5Bq01%2BbjITyD6ZmySjXAPSUD5s8RBN6gkKkjm8o%2FEeSDVYfuKLk8uwb%2BVn2NINDivofMRrsbJ08ma3srYcBOp679oc%2FpHw9O1hgyXkPChgjNzPME02S2IimCFEzQMIT0%2FscGOqUB65J4x50qkv8K5RnycMQ1Pqv6X8oak5T5c1hbyl7o13xrcQGdihc1O8dBnzNjYwwYWZ7yPQmgalFq7ucawsfvONBmDpurKn4XS%2FW9vZ3%2BwqshHpMVn0wVkmzSXULQJ5kUCFbhriACet88drqYfNriA5uSWX2txynDp7SDEC05OyUWaztYlm0cLomlM146vEnwhHKOipXDAMYfkYvvpHdNnswoJy3a&X-Amz-Signature=74951245fd5ecffc55e13eb8852878397ac0bf756e3a36f9dcfdcb94920f52e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

