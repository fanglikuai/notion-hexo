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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677UMPNKZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDhRCVosCAeIOe82WBGHltH%2BZCLDA5djryLN%2FgLtfX7NAIgQyLbTpnImb6gwfIuvrARBMqk2HqCJ6YQ9CSP2YpaI9IqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAmtKFy716R7mjBCjircAwpOs6AIRuHnzMkATLNUHkRPZl%2BIeBszhIVQWc72qdBsJtC4LQW5uVpIGXBk690VJug1Nw51jXOnYFhhIL3Rj60fDDRaEPYjFXSpYqM%2Bmijz2jGa0vvpE%2F3mzKoaeumbmW%2BxlGeseYty0VAQ%2B9ItZQNSzGmOsMPgTHSQn9SoXUwEna%2FHE4V5DpqQ0KE5n4UHwMUlE9t25qRq5A2gO2%2B8GyxKPicKW0iV%2F0RzuAmSTbAV1SQXwmip0k8Bg93jEDO9UMOBb7A2w7HphwrMPm8aO%2FuROAviCrQorE%2BQ6YGQhgCFj7evXH4En4mHRJYma%2FmIUJ9vs3IVrhibG6j0YqDoZB%2Bys8F8PR6LcdGBJza%2BoyophaIyRYOAC0GUyBp2Q8OFln%2BR6hM8YbU2%2FL0sfgGUfk3pn1%2FwnWDJzh2l8Va7Kis5M7rrBApHjMUgLl7Yw%2F8%2FN1eQGZFwkFLOVxtzqc2nFE2Ddb6wJAuXzEt7FwWLET6hctE4UsDmFk03WgnZGFkD58vzz5xVLcHltXyDIv%2BlsuwwbzzMvzglRjmWCbEgcJ3b3UllXQIlrbch%2BJ91WfEyLy5tm2skqAdIjLHJzKF71%2BCtZDUExvRkKSF7ICAkYEG%2BwWn2YZqpQyJgR54EMMii4sgGOqUBxn%2FRSdE9gCDTJbK0Fuogd%2BDvp6LMhznh79i9TwvlOVBZtvcXt1sYDsJTzqLB1a0gyTXXDCrvuFa8gqnKNl7hHarWbWP1uZj3Oe1AiqETS1ZAcpFgJmN7JkMtfbczyE%2B%2BqvWqon1HOoVw6aRMErqKiQ5dA6puOBIUPXdhLpJvNimeIxrciL6hnyV4pjLfINhDBSanZZa4RN%2BuB9v6aj7bCvYhuMEu&X-Amz-Signature=060fe44cbf16f7a179288fe9791d1e6c0359a84ac0a99c8a5bb5de9e0d987a94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

