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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HKQBBLO%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T180108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHWkO1UlxR0Uzm4RoU1SEY3BOdzmktNBp3WNwzVZWRmxAiAzDNbF6peXjSnWLfqXPsnzmG%2B%2FAXgVjGNvysvR2G%2F%2FqiqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrIGVjzXRUSQUoQr7KtwDwIELMTPYpLVxWi1Ae5cPJsK8NYZwXdq8jOTOZMhtsLAGgj82HkNm8aHA8W3%2FKGVuBq6zmBmRZQi2B0etKrhzlAZwl2v0oYc2HcuGzMAv0KRjomn1NMQ4CjraKCqnx4QTp2yqZLdIGr0M9a0Y%2F1XmF2J4OF03hK7bOaw%2FuMGbaoP3%2FmFhbabkEqC42IOtyvtSJN6jLZEMgmXPBEFB%2ByZOq9GjPA6ghvRk2Z00Ajn5CrDHmL8xVK3EFWGcH8Le7220k8bhTlr6POBilXRgvrDnQO6eCwhP8MzkwdUo2fM5F1tlB6MmB945NZH8CTGs%2BMrnF5OarXmW2EVwMultYTPFLvJyIzaylBUM%2FMXml4uDa2rZQIbyjLNbkMAq3LuIsk7id4rFfN%2FLnBt4xWtzBGgGm8zVeaNDjxKJkAy3okfYaTRRkeqLQOKO%2FzhX%2F7W9z0xYbTxcxeckc5ZQ9T4mtmmO7QImLm2X4rDckdhV4Z5JSStCON2um1tGBNozcDlnG83ZnSP5OJeuDYWORXqS5VpR6bdFhLUqt83%2BOFFPvLP2BLVsLKsBSBidMvy2HiGJn%2F4cHKIs5%2Ba8UVpZQb2WPHdVEXEBe19CFSDXLSDF5k%2F3L3vc2dJKcImVRMKbslswwdTqxgY6pgFtxHuZ635w4U0gKmXsU8NV19Hz7yKtFV3s3Wc84gr4%2BvG1So%2BDbTwkTESEpFYH1%2FCUvcQ%2FCbiPcvR%2B%2BVgeVVgQap2jOEx%2FR9tBaiLAueQR0bO9LdIjGqAK2I2W1cgaSSVmfSWsT84hSong0EovzlnJU53SaTfWUgPfRJt8ASnXxd%2F%2BmXDuEpWPHzYthR4%2FijwvDJgMl%2Fx653L134LdzUFaMjmECuLS&X-Amz-Signature=a6f7525c5fe50effe9563fde3fc09e1e6db6c05b28971395ead217143b03b015&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

