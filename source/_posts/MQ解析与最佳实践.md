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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFSMYXL6%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T120048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJHMEUCIQC3CV3BU7bVGTuDi6RwAoGQfWnLdALt7N72m5kEv3hJgQIgVO7wWB6wKInPmDu2YXjtrgff%2BKp4cZoUZTk6NSQ9tiwq%2FwMIBBAAGgw2Mzc0MjMxODM4MDUiDGrowzDryHNnsKHeVSrcA%2Bajd%2Fw5tS3P4g%2FiMiBS5iKtC9nDi6%2FgsfJaIPg9joYXJW3xthIb%2FRFY%2B932Z58MhWUm29mkcLwObriAMpuxLMTJrZFkhxvX2WAbWQ%2BqeQtmeBF9s%2BWwOTthRLDFGPUbHw7Ot%2BjRxJsWSUoOF%2FXq9UDtVuVP9ljJFWqtdryy3pdjllYnZrPsi1pl9bZ%2BtqHMFTHmP1jhcGeKDg4R29X0yrjd8A3ce7cLABIbUZEdrsI9ObnUyz7oAbTsjIECDK5JOr5Vw7FxR1gjniyh1yfypw6ogsIqOISN7ihUdDa9VzWqoGZZn90yy4nPsPKrV4U0k5CSDke%2Bv1I8ub4gmjwhlR%2FYPdP3EKbG1UwkuDhC46IibS1dEg0t70a0rHqH7skTMSff91a%2B0I0fU4pQW5zRXbE%2FykR94tjg3QsEdaIRQq9Uw75S53MPey%2FxhEdepu8F30qEPtyk24TbrWvFb8zdnb6ALuAnzUhQp1liGtwmFUStexn1OX5Q6L1NJCO5OtNnmlW2T2SjSixWyIGNxnDgxcr4kKl%2Fu004bI%2BncBA6RBvIWzJ2UTE6wa3t4IYAlteTVb5Upne8dAyYEJdURVFdX5x2ZNP5NJgsSdrfT5mHq6Yw9sjJjKXtXW0PhIC6MI2Mx8gGOqUBRzbzC0iEmMOHAcu6w8Il%2FLmniA8jNkW%2FI4jX54oc9fR4Ujfiy57o3r9u%2F2Z41ufmyA1B9PeK71XON1fHpmgmkQFuJJ%2Fxq5dFi0ywh3hFX4GVskCyerVMR7HVuIaNAk8gN0zv0ctDFvOyAa3VVQIgBAGgL8kb%2FtRK1T4cDbGoOHkUcyZ0dkkuNOyB2TExf%2BqLH2pqWVisXlUGRdlp4R%2FE%2Bor1KsXF&X-Amz-Signature=090cd6d366981d17c7e880ce9ce39ff1edb143e7d0b9165a7a032571333e19d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

