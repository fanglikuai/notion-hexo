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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZWK2PPR%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJGMEQCICKojMqtLg8WNCjTPf%2B6x%2BfA7XP9mXYsV9%2FJ2vGXrmETAiBjALNTPRVbWeHVoSFwOhOMGHCNtdOJWF%2BZhVc%2FKpMeSSr%2FAwgUEAAaDDYzNzQyMzE4MzgwNSIMUzQapnTiAud9%2Fk49KtwDvt9F90w4IrEPUwm0vPTmx4Y11dtHJsYJSFRCG0DAeeDwCUn0UTJI%2F5IqPRSbJYt%2BOZEBIivCa3iwEbBETBxaS4NL0KO%2BpMg2zD14ZWq4Yah3EHJGqBvcuJ84a%2B4BnlmVkNBNQxH4aVzRFEe8UvGrcp6QITMd3GRC2mIseP1g7Yi9F3ZFdzySylRoqwXiL0tvNyPYmrv7qijAxyOXfKmqwIKrxOqC2GcZCg0KF2W%2FAzQEbhyi2tjWlD505E4LEcEgU0ccLRop0yLqil21YKVtb5zu1o%2Bt%2FTCzh%2BdsCBpOxR%2FwROOq3jsN2Urxjg8QeaGA3eyuHPb%2F5TRZwzHaWk7bF4fLCHuDuIteqFvf9UdjUhSdqx%2BN9KgTLAlUtw%2BEVnJ2PMXfvx%2BpAWD%2F1EgupBmr8VYbXH46EiGx%2BxmDI0LNtgE3JDjdUH3pO6f9WYYMKSZDuhCmAuzyjdFmMBDG7ZCOYAyQXWu8TJJbPaKGhTfj9TrHrmev9J86s1GvIxhBHPMozgaCRo2x71nepJpYco7OoRWwVgot6rgZqtpZZpMfMkLcZkjnAZ02%2FrEnkk3WypAmZ3gIMia0q510w7fQC1gd5HqT7m45aM7Rby4gj9g2%2FQG0ehg3HeOUdfqjG80w8cfKyAY6pgHYknp4Sm8lHJ6yk4XWRupNFxIwLH7O596cIWHATjjVmiQbFoGXxY2m5TPlMORYFzJZDhZAZc4xl4x8apye96O7EaWiOLAZe48QIiRYn6jeHGsH7KErSnOgCvVbq%2BF1g9caRuQp9rORvIv59sIk0A6SJCd6x1Zaoy05t4YJDVZyMcKXLyKRXw2dPsn35whiedKn9MuGJwmSXaTcmpWB8iviTANjmU7r&X-Amz-Signature=2d785ae80b4a856854b8f6c6a82b4e4f1bcc2d956319065383ecf1bdd4618e15&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

