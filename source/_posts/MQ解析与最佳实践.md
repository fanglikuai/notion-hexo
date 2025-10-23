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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662B7JXDTF%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T100054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3%2FgEE%2Bvbqrm9b47DblIxD6n0Wd3BqTtqAgnxgJ%2BSnqgIgQ6LCxdR3Vi5eZyCMTzn7qnFbT9v4yrLGvFJ8O9Y%2FpQ4q%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDJQAYtk51HuRQisDGSrcA5Z0jHORu1%2BZO4s3EMdObFEapr0UwACyvfhgC2wiiVFnv5K2ciUisodf%2FkABJ8qEW2Big6wT1vAcHvI8LMMKCG8dfMtpO1qvHQlrFFjjPkbaQ9GpZEm7B3MuqxPmbRLiimzSVwTODPKjuORPwN%2BkbrC4RcJV44FaN10UecL%2FA32fEEM9NMMwx3jztMmZyj5HO3ZNl%2FWfIfdkyDdUIROU8P%2FgkXJrozK%2FVRF1UtMRjolFUv%2F%2FYHSCUC7szA1ErKdlIQiVE%2Bd8PlpRc0IOPqgV030md%2BpR5y0s18yFr9BorVKIo59VHwt%2BsxitQK7DNXM9CmPYisivzXNrimXa3WHpbg5z7SN1ZQoyxYMmfZaONbQ3nmvgM0Vb%2BQyMkzBWmano4pGAd1Jkqel5uPgvDYlS6JaOz7T40BHoKS2yEcucevdLST%2BS9RWKbDF%2FWhedMvrNy9OGgEW2TowXK9qjYB7y559DbkFxIBBkfvYtl2fehz8YI%2Fm2TUxe78%2FBuXhznlJ%2BwBU0xOMvJZPjCr6lr90kdE26XUmFgV%2B%2Be%2BfCu5t3jPbY%2Bal3k0rAyKxJZGjSke5Kq9LbSO4BGkx7vRIJB7TFqooHfqFn68Du1QGYOWONfPyzciJZcEmVGaexDbZBMMb258cGOqUB8XCzDnqRTKyD5Yn%2F0glDuLW4SQlQhR1Qz4cFP5pVhl5uBagUrvtSCZBiCJMppUBxHxeAwCSz%2BZPr91FGPt%2Fv6H6gJemP1lhAI1brb%2B3xJBiGh%2FCYRjLFipO4Jr3MMU%2FGtL2wNvqoWiNbVn9L25PTfmgCGvAL%2BE%2FhaIvIcMsFLCf0TSYN3U4tpTPybh4w9kprLySjTNx1YikxnXLnWJyEZMVxIf6N&X-Amz-Signature=b6864ac74b42862c5f13ba702eeb2d814eda40d1132dd98fcdfe4d4e3b13f0ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

