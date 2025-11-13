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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBQTHJHK%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T020051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJGMEQCIGv%2BiNdE66S5YIF4IKzI4NgKZe1yHOonfcDi6vSawSemAiAqQvontBG6b%2BKpxeGMlBNrucMyW4TEW26OkJenJ5S3rir%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIM%2BmToYV05dHcu8Y0BKtwD8wD%2FDZtUuQpD6mPtJ3Zj9Z3Nyyl7D2OW1fjYI0bNavuUWah5Y9gtqwDpiw41ION%2F9SVLFQhm0%2BEFqdTyk5dfQK6s7Zu6svNRGPbWGA%2F3S8Gj3Y7LghII6SFuKZfyHPBN3ZGNwGLm%2Fc7tzZ9OMQOsX3X%2BwtcXttLIvw8PoPNofun5xFSbXDSHBNEwwYnIzSvbFzpuouo2jrQVv%2Fc1bPKJrvdnxsRs9Fv8lYlT%2FGkMBvcDUzUAE8gfc6xKNfaSl9s1pqlLiScHdJV5lLpkP2hPo1OEdMfbOX6wACaiqImL%2BQO21iqHZJDbBHW3pTXAwtdDUvoTvCeNtDbnMULDOoWRYtH9eft1ccTGuN%2Bi0n4pKS6LqPcoU0Na4zqUrHt9j1RbizcoM%2B3v%2FFFVBbdiAJrj9rsJNs%2BnWMUPrCCys00xBAobEldjUKEqTH9JXPsPZYBL2W9RkD8TITGEnrxR%2Bk7jvZQeBMBsIhkwYILOHPs0vZ72JgmARyEQV3kmHfRbApfjEJAUu1j0grxfUCz2N9%2FKjW0bE6H8jhrWhm%2B6cU%2By6g4CLxFxCcGrx9lHiGDG4lJ7XOsw5PqhZXIED3VvwUXH%2Fm8tN9%2BgQF838EbH5x9MscMujLM%2F72Lwd5bc%2FzkwptnUyAY6pgE9HOoiLW36asjFK%2FyHEy%2FSSFXRUc%2F7U7XJxCIS9DH42zWCCo3iy%2BZZgd13a5l%2Flm2if33ypIPuGvEov1ib2XfJL2SwH7Dv8xk%2FU7RMK14c09Iaw0K81sY5pO52N2DwZX0ttpJhIokcq0uOvWf5G8MgZq%2BfwZVy2wPodx3BKsw00llLi%2Bd0ZBxrkAD1F7hgFGmianestmqRS0ZIboxS%2By7%2BPJv%2BUbJg&X-Amz-Signature=c6c8202861fce474def50363697c380b94fdc8a8e6aa7f6ef80a129ef67a05a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

