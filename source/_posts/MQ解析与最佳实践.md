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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3FCQKAN%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCICP7RqtV7qj957v0qZ0GxreuEP06NLJoKHjFbh3q39NKAiEAlRnTJoVUM%2BNUzZ5XdEfQ3ozLeZYceoLBAVBPtOgJCDsqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF9bPm63QkTpKoQWJSrcA1n3UoyC2UMGdK6ABFqieVolAWHNDWsqDMEGns8%2FKdzmms1b9JkjAWbP2G1emPg8MoSUuV7K1e0qYTTObvoB8YOb3WvQ5W89FjUZJU%2FD3hJ0meondfOiQT4wuIKohDZsU8JO2BRaiK7HK48pG8%2FSSCF%2F7aHaWVWJS07YPfiRdru4Q1jUakMXv6bcg9W6KO7Bpc953V8EikBvcdDh4mJ%2FO8PVYKO1p%2FnJhIvh7r8LWsDMlSlCT4XyEg9DJeD6C5OncoMkXe8SVj97ZTRQjc5VEar4E1A1VTncjRCK6m12VF0YlmuQUiDRc0YOoqp2IrzAijGJdVchic4ppCQcHIXpXS%2FaxxxC7YF2cuJFinkekUyaNQbVXRw2J%2Fs4sAV9P5kuhHykLxeY%2F9GsxXPB3Cdoc%2BszK4uQr8UKjItZ2%2FtgaYD1%2B0O8A%2FeCKovL2lUCkZLI40wcGeSe%2Bgv84pgh%2F8MlXYF9%2BGeE3sMUDb81Usuc3Q3AGkeBOoyC2TCD0VHqA7QAUPVbEYZ8CdqpCqjfhL0%2BkawFB%2BIKyWmJ0j5xat41hUbaw4cY94VaAFdTFd9LopQLRFKw%2BCMOwZ%2FRx3hlyh9PiH28JyXlYB6POrc3WJ%2BstFH%2BpcE6c5ivGZVPBCmmMPmYpsYGOqUBliJ7CMAwCSLaKilqp248mWIfPAXBj9pISkYlphCrWuoUJKsEg01WM9ef9QPDuOWt5Mmaz63WLAVBJyupcK8CiW97Zs9lvJ4wI1O2BF%2BxfELlaYPoC6wGGSBzbQSVGmuG6aWPR2yTP2cq2P0SD%2FawiAewyCdmnXPpIEbXBLL7cnOsqczcONrQCXwLG2XGsFnnd7oAI06tL5yXZxbDLffmIDoY5%2FdT&X-Amz-Signature=9ff8632ac1a91c45dde2d1f3df704be80cce210ff51e5a57f1ddd78c51be08ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

