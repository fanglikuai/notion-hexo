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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665GRJB5ZT%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T100052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD3FhPLgZGVo%2BVS5VxS1AEaxDeu%2FA6gz2I4%2FzhZ86GbiAIhAPZF8YYUIiEDsKrFy3lHAv01IGCYsEDvHPUJeNfDPCbJKv8DCHAQABoMNjM3NDIzMTgzODA1IgyHpI%2FZeOMzCX1W%2BsEq3AMGzPhOJMjK1bg1SgAyfE0Fv8sbKkGKs2iAB4BIGj36biLAxXqvz4LVKdyttwxFri7F3ymb6oYtu%2Fc3CCZNvsbZLOlkSyGbvs%2FFskI%2BwqamxuPYhF7g8pF6cS%2BRTQjclMAwZQ2VTZx9AGVWMbBOWOgRj%2Bu6Bzlceo7CsaCnGUILFyX%2Fu%2B2M1VBhmq52UYGlcMnw9oCIvd%2F9meXkhFZAHkGwP6TGQpLmjKTF1VNdixdRjgnC7cfQ3%2BbKAY6AuzJvetjcpsI38lkF7edsKMq1OQ29qztYQBK8%2BDdGYwAxVTC2Ks15Zz09RZssArwpgjhqtLyZ2xFMJeydWHk5Ls5K8dHEozxAJ7j%2BgGYfPQZI%2FAhjWRd4BblMe7BJ9%2FHFARYQQ9zEbkj0dCkvgSF2FGYgnfiBdV93DPrXzQJ3EpS2QEt72DguziSwfQbHMYtpcHA0ncUatZcFWD4KQJRVnGnkO2i0y7kqk0ff9QDdVFRCo%2FAOcBMOdZew9AJXbVKv%2FwTtJfHwMe0OT7gyv5QZvehBRcBdlRbmKKk87D8Grrzg%2FI7LeTqne0%2Bx88gpj61f1DTiJIo%2FKnXdABaEh3TnVkWXk3A5S0i0A5nvnqDah7XgfAJoZtf0dfqTsA%2Fx4k4eGTDn6vHHBjqkAdkfH1khIlFzmN7QCmjzwuxVCqNrpD14MWb5Tm1tuIGUdxd9gdaNA2B2VB2PCtwasuSjq6Z%2FssMdNblyQsBmeskG59UlNZQ7HmHk6Kt9YYxjk6yDbsQN3WssAKrSZL9PYGiQw5qNSqGJA5LRVwxVkm5fja2UbJFY8Te0AuAHizoafn8rVT3mfgLzla1qQfDjKwGO2D0RzlGFwnW17s8bVgiEYmrz&X-Amz-Signature=eb85ae9ae05289171cfdc85b587c15e116c51bcab4a2e25ead3a8f125adf5370&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

