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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664MCXQHK6%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T090043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDpO4EJcgDOOtPIu81raIBT3lRwRuDvd5MLieP%2FS%2FexngIgEZKBAv2XYS5uelzI09w1GQ%2F%2BIJI6t%2FAEeZBK%2FIt9n9cq%2FwMIKRAAGgw2Mzc0MjMxODM4MDUiDFNmb%2FnYs%2Bpn4d198CrcA7IQZdO2cVdBlSpMjrzKjwrhQHorohfyef02zxsHLV2j4LWILRRZO8i3%2F72uQ731DZ7d%2BHsjuz80ndNc7gBy11v8TJLKpqeRJAQskwiT1LygnxCs2ugWThLzbOgoVMYTUSOlteNSxMUajn2BQbKpKd9QBXd%2Bz53yEvCVtZMuUfJAMcRxYrEl%2F8MwI2EmO9SmonUzv7OwvzIePSFMjOOEYMqgGBvJOILCLXwFSKLeUj%2F%2B%2FWmPayq1K8ErDi%2BTfe9CO3UvKcRZtzNKvMPtjP0xw4Bo6oMDsrDOtuNc9bwtrBv9I5GZlpWGMwv%2BAQzlL8aOfJ18tk0egGm0QHLk%2BEXmcWEXn9iRtK6OGaM8oAIE6wpKiBsHSx56aZmVs6vKIWsToaO78QHaJttx6mKL9w5079oDAYmDzXxsJPkAukf9v9I8nGLgWiEmB8kbJRCmmftEt9eWF5haGKzi9upLczrfEwlg1%2BkNLjViyb3Syjjgil7op1DYDG58YRIxtOa8cLtyqvbAT%2Bpyjo82w4Bb%2BbVTMhHcuOqrH8YR3bb31o2ZNTJs46Dr4uvrrdlc7KA%2FPysvRfvDT%2Bek5Q2P8jYtMRfuufEE1SIZid8nkswltxAsclEPWQGGFxYmKS%2Bbzp%2F8MI7u%2BMYGOqUB7ecPulxFyZtYwy%2BHPlTZ7YzcR%2FOEHYT8MfvkeeN7gpKSiexmx7xu0xVJkJAUwmYR%2FJNMhI7hxY1GZ5BnG7uR5g%2FACZoHSmec9QNJ53Z3KFlQ6QLfg8luhFTC6IYWG%2BxHgVLhvQzKatgz1tESgYvtbdhITM7qDY0fec2lvbazS0cQB1G7%2F1HyuQ6cmA6fdXB3LKFMzZMaDfWzIOMQiFAL3PLyhkC%2F&X-Amz-Signature=31a620de4476378cead484952f138deb0e57b7621f02de5e83934f90a9cecc92&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

