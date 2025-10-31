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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662KF6JAR7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIHVvEMns%2BimOVd9Ov12nSmhK%2Bpdu1iEiR8lAPzlQoQcFAiEApIh5ruJSW%2BJ5ErF5sIOreGRsJdG7aPJu1h2JIP55tEIq%2FwMIHRAAGgw2Mzc0MjMxODM4MDUiDFNqyuLbMdtRn4gVpSrcAwcmnraMoIbhZYwEQ6PrfSLItEUjEJA1o7%2FSbuss%2B26O0ZoBhEGsvvU5mm4CD%2FNrH0Q7iyXxLS4Oeyqd0qa4AC4kD46u%2FYDk%2FmhybyDx9eMBLhcnk8PWF%2B0%2BGoUCgFhrODNF4WsSi2Sd33%2BJlwOZwv7bneJTjJjqeJ6kkXoChquP0%2Bm%2BbBV2Gbeg9iAjgm8STdO76fs4NkjtMR7CtoRdcBMw5hoNzpITr28PU9%2Fyf5RaVBnmHOWdHXPN%2FurVfxEMzUb1KvKITlNdkbkaZ6XIuv6axsQgNxlfYNZo23WyNnmtqZrcqIZ%2FxQEtfCvw27lQ%2BbSeTPQEJL7HvGC6n6343WcWYn%2F8beRvTkLgV%2FC%2FCKnHOCLKS9jL7PYndBolErrJ%2FE6HWwuF6Am7L4AuQvMY7LW4EFWPEfLk1neJMePy3UWPDnryJ0MiZSphcK4InYAtpxH39nYjmxIz2uWwpRgXk%2Fvw3PZD4K24Uz5A43HBpRbNNMAch9szasDa0MDFKM99ZsMM1r%2FRuiisR9Fox4DHPzoYHHkaA958erpgIIIrvxrFx8txJvGkZEk0DcoFAaWNCib8zTigSEW7Be%2B1IVlmoNcJkOhLh1sJmzzyxAC5WmPJhuhtTYtGgxVjq7EnMKOilMgGOqUBohA0WEegpRFcSEJKp06%2B1z6Ps%2B%2BQGcDR%2Fe%2Bdpvji8AtzVUgpg%2BsPVM9z5WuR%2BxOu2nljBSsIw8Z%2BuJSSpb58xfvRK2Aqd1t2XyW2lz8OgElK19iWu5PhYSX09no9uaSe6EuozC1Lg8WF5ATGlSZyICZ9hTt%2BFeekfJmmQ3NEYEow%2FOr3h7%2BG9GDE4m9CBkiuH3QoTwGziWFLLWtFh8u9vHjWL2Ck&X-Amz-Signature=d2dfadd52c74dc85f76f0a1d3b6fd1335a55aa256e60eb90949e914de6956a83&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

