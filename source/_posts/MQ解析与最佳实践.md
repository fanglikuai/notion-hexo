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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIZFTQE7%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD4SMhQECTyllu%2BJfJ1lmS6lRka9JWea8kIOnP7QsKskQIgQNzTw8WfHYB6k65GgsS1kvrLKYZNK%2F%2F5ZPv7PVvaeGQqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFcHS9pCeFEo8tMRwircAx%2F46xMZp11WfmblAB7pS1nQM32XQ%2BJeJmWymtZn%2FX10PaHG8pcysr4IcW0Vnlqnngiu%2FIapIZO6b4Vd02PixGzzCl9kdcIIIVIlXtosAy9OMcX8EfivUmQmT%2F%2BuQ8C0sR1ckQMqaP2PR0XVT4ZTTh6JMG5Nze2XC6P3fj9eHKZR86qFuWCNzuu6uB%2Fhl7DN8nPdFGzcDkpiMYVFQ3MqdN1Qs47baysLbKE6vCK3jPCtLaSHsODjLniKZ5fOPhMF3PIzY4UBvFoZvwGCeINhsH%2BfJtcmn2l2kAtkMAsDI%2FvGbGDbcCGq5ru4P5SyS1WrM%2B2vdSIs%2BBqJQALaLCbxlviltpkyNsrSmcVtlwpppoIxvOze%2Fx3HtZmPO0Nb%2B%2FTwPID9uIf9SxLCUWZQvpOos3DD52aORiQgwILQnJtwMUwvsao9peK8Ng94wVoHCDBIl9fnE%2Fz9glPQasI4f%2BEsOsfpTkdSVlGz5nBKNboZd9j8oicvGAoUlRtrYLdSW2RwtoPQZSmmzq0yfis1A4vb%2BdGcuUgJpsHT0VXkVOpsKseoT%2FXlU9CY1rA%2BCY4Q66WYtpfweAknbRBKOPF02%2FB7ChgwXeHa0YlJSQwW2f6RNak5L8dQCTiDmILrfYKFMPDv9ccGOqUBGEJZ4TG0I0llbJmcj3tLyTGnFqPqIsvI3r6yV5Eov5Q%2BX7wlsib4cwt4Bn1HqHpuY20Kru5SydzcXFpsoiOCjD9sXfT7EXnaG6O47AcuHdS9iNA9P2Fuh2vOsS%2BOMT4TaMq6QSNmg2jGln2p%2BKXsmxzoMTK3CpFl4wlGYfq5V7r336BZDP3jB%2F0AQpjWNSsoLX0KDivi9P%2BOpPa50lbKDtRU3ZAw&X-Amz-Signature=e97f5db5e794b7704208727b2c255d174ff812fbd33293b3e3fbecdf893a5acb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

