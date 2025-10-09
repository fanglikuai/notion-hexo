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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SHYLSSTH%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T150209Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQC1XokbtfcRcwckDO0T%2FeioUCw0hNEW4qRuspStP7cMPQIgWsdHKicq6u7X9VJxX3Skf80s4oZ6ynC4H%2ByvGniA7%2FMqiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ%2FYw%2BLvArwJY3%2FnjircA5%2BkNdraoubjYSua%2Bp%2B9DlTvF7MEKh4cdceg8HgXgUW1I3xpzUMNawAfLPQlmOayaZNrGaBGY7sBX1PMMaz0o8NkjhScaHFf970vaXkqhaanoZG5w9LorPw6zusITnHx1%2FvTwJcrzPe8mnT2jT7kQJMFtxESiEXHJL5yKOkrbWVzvmbKpaF3tGWmgdRIOa8H7oK%2FlRwGOSUBcJzP2pplsvWgRGo%2BCBrxoONFcKXsLTGdFEXJxfGZYEQ8cyfgRLO5qxYEY4ZjI%2BWIWhV7NhN2%2FiDvrbjsA7Cate5Hl5XY4G0we%2BN%2FXDgHY4UDu60g%2BY9dPEqbc0OWOtoF2bVnIVXWUnSBCywq%2BAGTVMqQBN%2BqpCyYTv1cb8v4iRpW2IeVq3BmkcKFQ4EWjl4e%2FvUgO0xA2GH5IpCptSrrbgdFNWL%2BZ4%2FEgIdAljk%2BO9MZm%2F2EQNEtW3yAT7WTzlf6tg%2But34IHYsyWm0YTw90eUFLpfbuPiPvn04W80L6vjXDMiFRaH8VjRQ8ZKIJkAmqlnIDQ2HLmAcSe82Bfx%2BGNNQMjxcoY08ePO8KbNfu%2BNLtRhUD8Y0ey%2BGSubfP0Qoky2pc%2FVzvpy1AXfh2BgVpKIumpjwzT5XWT1OytxTQ66ZrDPmQMIiSn8cGOqUBcQNNT6DP31ETkMYaJSZ9MgfY5VnqNTtdLzZ15V5vElj%2FNmzAW%2FEMf4Z5lCIXLBLowYejJrLe2jf5Y5R8Jd%2BwFEy6DrRcHliUcnCwTlVQGcZYUuKaU1hHbaSLIj3%2BhNt3nGb20Cu54xAudtwMOoKePTbZH%2FtZL%2FAtJ6Sq5yuoYb9Ce9eIDXRUzSpkeX6XxYs0p%2FASZCVJiCaigUzoibNEEWQ2mrU3&X-Amz-Signature=88b48885a524d6980bf3f657bc9e32509a4d7a260b1751f011527b58cb242f36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

