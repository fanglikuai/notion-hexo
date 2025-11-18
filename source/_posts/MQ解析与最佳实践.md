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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJLUSFXK%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDN2%2FAOcqEQPRzCICGu%2FR5PqQpMHtsBkL1EABKC8QLECAiARywvBSajorq1dPdsi9GEPGsQIvwfrARrI7Jq4hmBlPyqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQARf5FdNZXY%2Fi1LrKtwDjIk%2B5TZZNoaaTcpHMc8YF%2FQoQ9ZhIGdovr12w1BJZXajLXdFftYd%2B%2Fic1cpv4KtTKs0SeohzJBtWcAQSGdETDSaglUVbJl%2BRcYMHwTa2%2FCC7bJqsqVe6G0CNaq88ZOxOsbK7ti4tUXncTB0bK7b8Fn%2FUJ504ZoS54JT5aC17woxE6iYPWjCCETC0%2FHOFUH5eHawwhVg%2Bbu4pKhoFr1BgeEiJdCT8v9E%2Bq%2B1yFM%2Bj2RgXHE56EMPcRvPhELocgWiB91dNikjG%2FE4B4oKHqW3btA5BcN%2B%2BJ9e35g8nVopUnX%2B5kpA%2B6%2BN%2FJcAF4whJI9TozNSxZ9TS0%2Box%2FRivcvQPJhNNpojDa7Z%2BwgOQ%2Bs%2Fc%2FCMQfcFig9E5Ar5J4Tlm5AyZC2S5KvkW51Xly%2BGu5Pc9swY3dqDsN9558%2FYdq5SdNk89gqV%2Bk%2BxUjq9hx43Y6SMmlgwWAoyw%2FbhVQDppMAOsqk4MaY5EwlAxgSUujJ988o%2Fjw%2FHkVIAjtTWl7M%2FwPrNOvDL7MQSzYnIQQmEuPb080dsbyUVOWzcTncVrDiaROpEi34jTo7T5mVX7qxaFzIu%2FyDfvWHx2ll2GPJLx9aOM8fq3JrngTZrgXCXZvmCM2IHK2PvlyfSW%2FZngdAEwqL%2FvyAY6pgHQGupMk400mlVTI12N6XTuBpI3CdiNCM6k82yCk5z%2BWErHT2iEdwGkzx1rBkuqzynbrLKmRzgm76%2Fdf%2FUXnN7A3wN7cRkeZyNVXthcboQZ5EDnLu7L2lsIJmDxmOHM5CKLKmOt13H1pCweFjG4S6dGt6eDc8u9R3jCwx4h7mbRTGacZSXDXwOESLd0UJz1GPWxrRm9dmgw78SsFdkQd7GcUVu2qGVp&X-Amz-Signature=ce084a9dc756891c6a6241724752a201af5c106d78709ee9184c9b8170e0b1a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

