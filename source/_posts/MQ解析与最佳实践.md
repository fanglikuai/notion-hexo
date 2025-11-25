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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662I4IZ3IF%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAyZmNJ%2Bwqo7Cd6GhEvI8EvGb7ZOXV8wcDyPxzIRXdw4AiEAuLMy8CcJl0JOZJjo20YL6%2Fa8u5owmdA1QZdLfVNT%2Flkq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDPjo6%2BJQCFVbzh2qECrcA874dUElUaeKjCAqNv%2FCTZgOGIzvtr52VHavvrsylsgNRmnvcWxiQBfHahOKShD1Zljdp9eqVlS%2F5lNtdmuShOa7%2F7yq92bmJm3Zwn3ndX1VIng8Nw6Kd5IrYuZCSY5Z9lx6Jp1qrMRjbvekmTBfGW2ZJRHla%2B4nXdV32Jyb9alD3lG1mjTlh3wWNL%2FSiyIFEOa933CSnU%2BZ9HreTgfbfvlFR3xW%2BgY4F4JwqTCy0wZvphs33NUvDTX%2B6gx5zut8BqgCTLnHe1ksaQTtu1SNbsFy4cbjI3hsCNyKFeU7hlRYkOJmfcVfEoD2ve8NyVHPLI5Hm%2F7b44xbNIEHqdS5uC89klLmJEP7mcXO3iSzLAUcK9DYh6u9EWMiRQZ0L8aFQFvFg3TaSAQ2HtCOV39MMgOyK89Z59PkrDf0Rfldu8wkLs%2Bsl8XB5hVhqZQy6nTi8Rt6I48Q%2FOfaY1frKIuZps2iuXL2pVrBPyP1uAYS52CZy%2F0CxMzg%2BV%2Br%2BkYvjMDy3nOC143%2Fh%2Bh%2BbvuWGQ%2BsDLwfCNKThYQfPB6XjBtJCNRX1BkaJnj1yhXxG0qMpCSsHI00tqqMyfYqWREHT%2Fiubi8uOzKFHgazDIOv52Gqr3Ug84Tc1DiEB83jXJmJMOe5l8kGOqUB%2BVLMgN5dkXNOHWPdhV7s1WZXEXZk789%2BlQTScohonItLtwTR4%2F06Kwr%2Fpey5y8iGoB1FQjs0g9qlEofmhBmGCtl0AzeqfmyqJ9BUZI3Q2J61x4YkI2TOxmVHFzmPUtPzBxRvEby445xOxSja5XpnPhwH8nEA%2F3aC0THp8Hz4HF2DS6O9dZOIRMBDO3fFiuniXBoCwR%2FFWSaJdcWWvtNrMggpydGr&X-Amz-Signature=bcfe4e3983ed6a229837172ed9d0ef7b43b9d0f80517b44d0938b0ae0721bfc1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

