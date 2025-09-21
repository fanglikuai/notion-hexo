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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HWMXX4T%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCZq56M8IyIRu6rvIWM0eMRxnn33edh79AwQrabVV5UVgIgDBKDE8SlG4XvM4plcYbAoEzoFj0DpISi0IKgjO6AzLUq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDIsUzF2Lc6uENZV2jircA87gW1qQbfxT5QedYLkmO4jB2fwBaArBiQ%2BqZA%2FDUAQpwNAiOT7H1EeinE72v643%2B0VcQXETEEV61nM9DQt4AGx9hvyXpYaYh7MlluztRDMyv0p2eD%2BaCmBUOxtVDIEyBe8uvbjWzITgL0mGv1ESks68pS0TDKpCEp3SIq1S%2F2iMLLH76lGbxZJwk4vcjTsSoPnl5T5Vd9PDQOtqfebvvpmlYRPLWjVi3ncgFGZrp%2F6j52sqkzW4ScXX%2BYug3LhBHs99nxwaML%2FSNrgTqqbSVYhSkJ3Ef0%2BJXPetMNLtmCW4Om4UDulsqVgcEp%2F%2B%2B06GU7DOnzPqjIoK%2BKjgaZQ4KJsPOafatEuM89f1aRrJFCkTbbglc4oLZQiD9tYxo2F0lh9CZIRI3GeGHn7fjX8kGhiVkMfQe4Y06K7OcRGMP7RBEtjEwR18Ifb6wzYo2UhGOu2si0VBGbSlJjajIBOLrX9F%2FUTJtcoY0ly0ynHQQyh7dDBApUFXOCRMPkl23nfYt4m7IbB1FYEIP69HKFsINQqQYjuW%2Fj3b%2BipvBPxlR%2FaNBEm0TRhp5JEQPEbZ80fvrZN%2ByIcBBGMb3uTJhfL0IkpD2AB7jyuwWkr5pg3LZRStdefVR2xn6%2BFHMVC8MKSZv8YGOqUBvV8Dzydmvgh8NUUXQI9W31v3it4WggPHmOmKsp5AVmraYEhJ7JB6ccfAldt9g4GO0Es5nRLwH9pi3pSK7Uck%2Bl57bm9T2Uz%2BFWSC20zRYsr54OZENFKnPm1fgW6gC2xsU4zII2rtSdFnmGHmheBcL4iCqvbO0XXcQSD0pP9P%2B1DSEuJEQAcuMcjKGfLEU12%2Biu0sGQEWw0LixqanjFkTZmugoLMG&X-Amz-Signature=750f8c26777510c98a8d5d838779c0aef9529ffa406d0d5d73622baa8b170aa5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

