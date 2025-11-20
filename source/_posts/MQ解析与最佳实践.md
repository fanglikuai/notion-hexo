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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SDCWQAGD%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T010057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIF9B%2FDW1d4wGlX9UgPGf7Itj3RyvamRn4raL8zeB1KWzAiEA1D%2BbYFgSQ%2B%2FllC49pEYwFWBrgKAJEJE4j%2F9TbQM5RzEqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOBZvBgQf7r%2Bx1fp1CrcA9FV6lZyDR33tXSnTYXnevIgziVzypV0e1TX3p4XPI67GVdoN6bVU4DsgDxUNSdw2G2kcwXBNJ32JPHuUC5BtAzFAlgQoplTF6%2FAtjksX4%2FJWOs3Qr3nqbbZNUX9aL7l7ScDWBXvmP29P9PS0qSXUo%2BWl39sMDtMGvnGofh0i4c9Jm6d1P%2FU6HanTdlR6kDezLKGH3DyNpbihSSWbJQT3CQfHPeKGc9kH7DYPcsdAb3W8Wamz28Ek1prnQ7oRkZJz5bmDh6fNvuWvrtM01XOGGHutFPHs%2FC%2FD5lmX8VFlWkvPf%2BnNBk8m3RxC3wYe2hVMQjOQsF42XpQbQOH%2FIkoP%2FQNWTA2lcW6WCXYGZeS%2BQ5uz822%2F%2BregprzEAWCLJKnMdplB4dXIJtE3krjBuAwLr8Vy3PKP9QechDb6qdpJX2saTDvAf6TOw3qkvpvfO0KRLbedNhGx4i9flfn%2FHRm8aDbsP25DpzCYMB5g9Rx1%2FhtnsxJJNkT4bBXC4TiJKhB0WTO%2BugkD8pNGb3IOtpfJXpEEcQvk6cY7%2FE5p7vWFk7FZHYTtumLhOw2dUAFnfMp1bPr0d2b8xLls%2FvmEm4bqtvKeF3py%2BcrpVW8K%2BYTrlP4VgbTdnIUVV38c3zzMJy4%2BcgGOqUBHRfooC04cHyHSoeJZAjuFlyxD9r70vSyvBDX2MKEq2%2Bqr00KZeS25HlWZgnTv4UEJgNeIZ5XW1OAFtuMPwpVuM4XQuPEFaMN%2FnSmmNPusBfeSZRj1H8dDwuhZqfm6mku8llaE1HkUEFC3V7AwMxas0%2BzPdIkbwH53AmN73g6uvunc3ZuVHh8KeKYmsob6lA9S4j935WA8egj%2BVqq%2FyKlBApdEb2R&X-Amz-Signature=38a7a2eb5e9941e965a1d247259d0ba04a7b450772a7ff3113f3ad17b44d30d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

