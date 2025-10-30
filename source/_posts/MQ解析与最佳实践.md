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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BP7VFWE%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJHMEUCIQC64Ws5HXHqZpDNczYEWSQOwfBrzmte58RrLQqBROT6mgIgWLA7R%2BOjaABK4VKLW8Q%2FJlWWvgnjKytIMpi9lM%2FhP2IqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM%2FkBejri7ctP0Od6ircA7FkN01U94xWmMwzby8bzffg%2BNzayZruskzfGgaXUiGgX0BRoxwptc5syYAWxDz%2Fn1UvlIVeDhY3oMG%2BT5rPN460Jd54uTqRLi7K9QxolIApWBGQi1MDpewQpExBMLHxupQoWNMVy23ha6ENR69FRcIPZgLBD7r6TJ%2FvSzEZ3rrq2dY%2By8Tgqy7Bx%2F8ohUAV0D8KAXHbwfOwB6Taov30n2377kYSWyy5QA1bKVBzYZGhDH7yHsh8UXOfkhZXhutwnCYttK9CQLBtyvMr7dJsQKWVQpXpLKuTMU%2Fov2MF%2BSOCnSvJEOUrLv4Uj1S49eJzuJXTmjWSDMtn2M8R4597RlOmmAk8D0PhDoNY66%2FLXdSTkbvO%2FqroudFb2wsArSaDk1ThWpiCT8Lmf%2FnMXNPqZqMmatfhNIaBXNr01bSWWij%2F%2BhLabkhtbTzVG41otbvbWAqn2eMdVKgEF20HhzALik1Fg4ugjctfYPv%2FQmq2bfZRIjaSaW1hYemgDGJbsQOGbYlK%2BvYiPxCHxwcHtIzg8HqzJ6vU7WAHSsT1lV5L944OpYo5jcxy%2FXnvMQMUxZ8vsn7GVPJz60StKAtLqG6vR0CZA%2FeWpdJ57JE4RKIMdCDG%2BBsJRTFJv35L%2FVu4MJq1jMgGOqUBk7m3%2BNLCBeul%2BjEHTf3jORwTuDjGA3bWilR90XaaD35PXjqQP0se8DVGLSV3TILBzZWh12cP9UqkYVjs%2BcxIIRgRYieRlNEe2CcthWugMx4azfDWJD0dgCC13UOVqg48Sc9%2FO4Thnwath%2Bif5CacgY3BxyXdmSyh99WVHayI1U%2BE0bWHHLSivrucP5GIwLrgSkzFBUGLrAyRPbhQglkugc%2Fg%2FXwA&X-Amz-Signature=74536956560ef401ddade3247c107790ea8c6cb1892c7080c1f7dc163d92def1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

