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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BNLV7OE%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDUFQY%2BIn0k%2Fmjxuk2SKMjQFDYLM%2Bb9KRJ%2Bu56gxOwdcAIgHwC24J6g4VOp8aZIuIdDbfZmZmaopDumRoWv8TSOBj8qiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLUtvWVOX1xCZjOrXyrcA%2FxqJRec5ih%2Fmxv5Fu5VmocCfTmmevh1WTjHywNm2ipClw%2BP3TSinTQCWIfJ3ZuB4FQ74koihpBFJDQDJ0EiCBOxjbhiSoOhTwNgVSLXMLbw4Hgk7GbFzHrpP1NR%2Bi3pz%2FllBcujA8X59zS1L5FOOW%2FOfAyA%2FWxrcPn%2FrRHxVpmeHPq4H5yTt4kd46WsmEQ5zyoWd9m2bpXAq%2B1OrS4IZCBEAyedoNyJnjhHbfKIJ7E2ipp7YispXd32DWY0%2FbZvhHUSNcXjN8PcObrvP6StSJkwNwkQOfTe8palnLRyA3Rjn5TjaCf3pYBhVVF9i%2Br1I%2FSLazMIwMP8rPmZnILhUozrBqdKc0zuh8JBNjDJ4Yy7N2CLNvbIp2vdbHAGXrOv%2Fj9xODpGLKAfzzmWDaA0K9VicBw1kQzKMxQtE2PkV4ipZE60aHeAbfq5r6sEAOSbaEd%2Fpki6ZMWfIIithX6Nix5MHuZ4rl%2FUtegYRZw7v7u2iELAOf7dwa6EvHrI1UeI2t2pTfmaUsXqI%2BTfoZey2jDRfLuZCfTLcTiUOQ0VOwzaj067Tvzm%2F%2FrIRaorKe6xmCTP7GySHpS0QqY9UHtUDORg5R2w0qaHNjOQ%2Fn0MY3xoIZSrJeeQ7b3soSrLMNmQ6sgGOqUBW5ZHiQ8a6MsLy3XLj0Ulvvk2psdKnG0g8MQG0v5L6y8gXY67lIc%2FkUP3h9HexusSnJp0VpjoLK%2BMeD8JSAjkl%2BOGc7%2FOH9pts2KegFQDwvq26ETvpupwFGOMwuVhSeczeIbQ0VhFHbfFTnIEl%2F3ZG%2BXckLczaiCh2ibF7m70B4F14HJIK841ZtKK577d0C1guoOTapU8800N8MfaC6wyscx5duxW&X-Amz-Signature=00a6c5498f32b490e459b6631444580b9b568c13ff480630a889cea15a412fc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

