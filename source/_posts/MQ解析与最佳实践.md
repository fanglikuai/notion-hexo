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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OMN2CBK%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC8TSlDbJtpa58omMHsbMPKoUQlEVk0qO7Z9VZ8Qxid%2FgIhAMRf94t8XeUifryoCnXFEQC4XSH7k4eHoulFh7A1m9CZKv8DCE4QABoMNjM3NDIzMTgzODA1IgxWhEdYUW2wbPDy1Poq3ANodHqO0qhlxlx69RwHiX1K01Cu1fORnnrzNIVz43exc%2FOPB65Fu1zHE864cIC%2FPwP913e%2FT9BbelxBz%2FO1xIJdxk19ckZM8G%2FktgMSf11c811O3nVvs65iNq5yGGuGxCPuLWnNngJcSjQHIGTc5Eb9E%2BKFVkycXnPxtoDQnn2CZSHLbMqJHs8BagJIjqQuZ4K9ErDWR%2BlsThYwuG2sXb66t0h7cenboQOFGqDpiLFuDxsYRVaqJ57gTwUKy0Xlya7Z%2BcNGbk6HnX7kBVmfFMEMAO8fQ7xXemcwqRxx%2FlDJeym4ScW6mTZU0O6XzMJ455IAX%2Buq%2F5Askukqr1DYpmYvLqgQDvODPTJzip6x3vRCeOxWc8WAe2ovUvWaiHSySCLzEEvPGwjRo4KGRxZDokcxBrndTptFzVR%2FCbCUVQMx1fCnyjDbmLJA05d5QuxxgU1zxzPXty0h6RXUnn5yjiKWNckkyt6APRPFcAj6NH5%2BloOnupeXkqaqhf4GcMlCyrIwGO6tSUTQBN%2BWC4KVFe0y9XXk7FU%2FVsvp2%2Fn4qnOZMq5rD1WGL87QZ1eJUOSuDrrK8%2B4prTUtI8umr0%2BZJGa2U8Rj%2Bm%2F2eY7SmZIqgWgaoP0bhxUy5eRgO%2Bw4fDCV%2Fp7IBjqkAet%2BmExmKQlnsrDVaHPHzuJvCrGHQlntNNFKk3GoSLVxV4fdDyN0tDD9kmV2oyr141%2BzKd9HoU0I2n%2F5vxBgrBdp27V1Yu016FMlZG6GOfPWBj9gIy6gLknRP4UjlPjn%2FQ57QnXwz7pi9syVtBcPRYb67kJje34WsASayVRRaJGWhvRWsnMtuMRFK3948Ee2Ljr3OiU6PAT2Lo3T4Zo3uf%2BNz9Q9&X-Amz-Signature=055b62ba34bae69df5977ab1717429048cf51817f6126b954d1990ccb72fafd2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

