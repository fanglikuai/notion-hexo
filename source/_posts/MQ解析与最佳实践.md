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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634JD4NXN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T170059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJHMEUCICiwbfm2eZmdu4xi3D7WWCnaXEhzIUtwotjXJysKVubAAiEAna1nxb5%2FVE93fRmHVk3Ap9rVQd5xECeJJyHgp9cmoKsq%2FwMIKRAAGgw2Mzc0MjMxODM4MDUiDIa4GuT5T%2FUtCoBQxCrcA1aU24UF0vWab81uO8fnMwpJWs7CTthrW0vXOmSq112Q7Fx5g47qfSILiJqYbyB9NubB682U7XEjZ74DkWTcvNfPYpfhzChmZW0eKeNN%2FCZ3ONgnlA95nsY9NeH6xS20kcidxoBlnZ4z%2BMkYova%2FaFjqlimfSbQOPW1AyKDXlKHx5sfunrxIMXjnficGz57jorZlodS1f0CaOsUxXBX2pgHA1DyM6HS0c4dcjocvN0AKdDHLi1woERua4suDxMtCQxeEFwnnLj9b%2FDL15YgeXta3ww%2BXLwf%2FvlXcbtXgQkn1l8VOJfkR%2Fg0%2B388igrzTNSaGbBZ3JdvL5bmGXJY2bWUhEqoswMAkO45gOLCP3EUAEhu2PP0dWSZVfYcERyXfUUATzvSNoXSzjtAJgu7RMGCG5sB3dEj1wUJf9vSvrk%2Btl5u51%2BowVPoaymHH2oudoOtwmV9%2FOimxUa%2FJ0dOCrKrxoO0xYdE%2FQLFq%2F8JKBKBD0PAtCIk1FGPV%2BUd2muO2QCtie5x4XFGK3Vp0tAZjszafssaTM9ATFWYYLOMYQk0X67%2BUFR9LRItB1JwK6M6HIXOBDF%2BJZDvacmXOT6t%2F5%2BzlSWRNjpmRx3iqUTBXWXuF4CLlUPWoNDxXENMeMLvDh8kGOqUBhibzYEnIjCypvaUYcbPxQAvu6HDhgJZhRfgRTAs1Av%2Fyt2bX5UzqfoYVFz9btWh0irTRkj0v6tHI1zG5C8wP4EZE7eKTc55056RNAq45bMNJ5RkGRFCjeBIF%2BQgl262FWSBjJc%2Fkn4la8ThZecI7i1hsit9FR3Cx0wsDCWAHWEhc4MC%2BMCOjyZz9lWS%2B9gvrsOC4kbl%2FZJdOydHgBPx8RSuq8MYy&X-Amz-Signature=03295cdd586d3a5de757733a80fa9f8c062949678c9e989dad9ca7a2824bfdff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

