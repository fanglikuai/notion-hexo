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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V4SUIJ62%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIFWmXh7DVIxRnqIFq70bBFKsuHGlbO2DvVR8B84zBq3fAiEA9YC4hBkaAg3AmjvMb8aoaT0agmWDeAX7rTjdHGyKcogqiAQI4P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI1LCHCGVY2Kk6Ts1yrcA9WaPn0F75KqKalkn8SoEWQRgveWH1GN%2Fg3CZNeyArNinzxqiHXaXY416zc8g7yVpXXYAOT9MoC2bDTk5oAz%2FKmdY8cuNAOtG0mLN858uGs5GgIzY94YAg1dSsyefaTdI58ksov8MI1P1FEqNFeb9PMdvrJde5Zn8rZWDlpE6Q%2FAz%2BvK27NGEJwbUSYZd8ZvIxLJuuOaDHnG%2Bidx3ZMk5bS5UpkEKKYH1jcZ%2BlBf1XaCT1OaiCTvMj26wrMk51%2FvwPgcwEzmPCqQ0LR6tmJ%2FojFNxuDEtOePLLhQm7QZn5pF09K2FYbyUP4WlOiWecnGMGb6%2FGDsmLWNl9SgnY%2BdPYhnYUtUTGf8C%2BXrHpG77On969UIB%2ByB%2FVRjRMQzSU9ysLv15DMbOhaeKZsqsZ6aSS39MFcmJZYPYZJV%2Ffqhm56%2FakE%2Bn%2FPmN8eDlQ%2BsjXvE3CFbtvF0eaTPgYf2aBhg%2FHtYcURXcL4krunqSC9PP7FostrNTh5gn0Px%2FAN8Fi8I0M%2Fdl2pjA%2FqaDEu7n8SNsGsbcaDHyfPvck5UlAhbeMBS2xqjAAiyfcUOHKKIRZbYzakBR59xBEJefud9kC9gzm4%2FY%2FBFOUS882%2FB6IQ82mBByovTXvMQUOSmFlZ6MIy8isgGOqUBvFxJSIoYJWYas7Ewz653HCycOX%2F8uy0IjxFddrb0YLCY7lLYnX4vuNQd9pNbyuTUztME8Ur0bXUSniV9HxUpw%2FLISStuL7DNQ1cBsA8UUGnRzieBwWt1JuFnr35BqCueclVeZTm5tAHHtWVC8i1Gsd04J1W0AG%2FBWWkqbxxl0Wvb0VP0nCZescyiVJ97e5jUakj0W%2B6KftidwcWOkt2BgaP8kjaV&X-Amz-Signature=75417782c0751cd80530ced791f5714f938ab3f62097fd60ecd6d65a1602c5b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

