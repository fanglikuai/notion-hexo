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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YKS7H2J%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJHMEUCIQD8%2B5DaO6qs8gRrPhu9607vYfbOT3TeaLKQRbsqFER5FAIgTvTb2eOnoPCs4ZBdowZ0uI5aXYZRGqG4%2ByG40m4iSTIqiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBs9RdtWB%2BDIyICc2CrcA49H3pFeLCGJrDeNC0cJgXetAm4GklFGK8chYu09Q%2FmFh1nHHWAkbHF4H1ezMQjPtdU8obMpWlEzfIcikhdMv8bQ3rGukBYqs%2Ffs74rznv%2FQvarBjTuGoUq%2FiBfLl55w1YlrcHrHMwA8zq%2BzNmkoqyKdTKdM%2BGGSzsBXWmo85zCzH2fD0Tqv%2FIUHIdoaqvnCu497LHeNKVZ1c%2BGNrgF%2BM2XXNDORXWyDTVdLPkH0Mxa2aK9RppZ5Zq%2F6PIcLmbORJ8PyHpif5Lj9%2B1Qw7%2FbEwNSaes5E72znE883nXW%2BiUGEoPiSR%2BXzsgSHKAO6y2yddJYyGWS5fs5vmB8INTBNgWmktFHt1DyUWGHRLayGoYkZ92X4h2l6%2BRFWHKqMgU2DhdV4evs6UK9zfX%2Bky06kIoXuqe%2BcpRESb5M8AU9avVw%2FEvhR4KCoKkoJQv9GF8tkccJ2ElK9aY5xs6UacL8Yh%2FeJjjgrQ9Z2P1Br04WdgcRtsjwV%2B5X6rSrFXy7fbYdSiZ4K%2BiUBuDLp11uZxZnwr0jjSvPQ%2F3HvCxdJslPKlsAB0kAR2uTcpamkfckjcDU2HVUTLvbkYbsKgiYwzTULSsynJTM%2FqqC1gihWOwx%2Fyj8CVvTmMOLzTG2nvH6GMLfR3MYGOqUB7gcPu3GF%2BmwYSSezVlQ0KxEBBjcbrFfDBId0%2By%2FVvd7csM6feE5FLcbIchdbzIArl5M4idlpJSK8Qg19Ah0vjZpJyi0UZRFE0RAWGUR8VvYvBCb3TqRfXQe7wEzWfQ3pJWxp5SqLj6VRHQCL9TuS41gd86DmT6oi%2F%2FOV4RZoCfZdBT77SsRLbkuAi9jxrSp%2BKxX%2BtVSMV5g8Wa5s%2BlTCU0d3bViH&X-Amz-Signature=de49729575000f5abc50f62518eb1ce818366b830a9e7697ca5d15ba11fe6822&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

