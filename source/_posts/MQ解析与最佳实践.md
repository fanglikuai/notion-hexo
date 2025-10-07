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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZPDMDJN%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T090055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJHMEUCIQChzKR3YWhJ37EJzg9aLLXf8RECQw1khLdSKjeEoWpnEgIgeX8u1XzKfH4SE2YbLozn2aRvLluD69lbDkPY6qcpBcYqiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDABHndnKmizN%2B50uECrcAzuIVgT8%2BiCjw%2FUd1Hd%2FiQVL03Regs6aw6hLCeCzA4fgp00B0vuXMfCOguGucyCWNGPD%2FPdcanNmYfUvskkbMy%2FwV%2F3k6k9HzFEI3c11S49i1v5HhK9JRqblECXuI8D%2B%2F5ezPVp%2FFBFHEVgg9EURC%2FIyxoQayhruinRRZAPu9f1GtDt3%2Bu%2FvrBPHSFSn%2F31WPinng69y6FsGveC5fOVN5QvB8U88PUcTCO%2Fv27UpgOt8IRvuaJqOQVAPHdAYXjUh7cd6ehngjbsTxTIUdvBWQiH7l4swmdZYzPFhvSbt%2FErHF7TOqPUaWHDQ7UuRbwPIjVv8pPECm5I0xGbw5gZg1Kj2eYsFfi2T9SrbZo9414uppxTnzVLPgrf8q2dW13cL6nRMzdLX0LcQ6T%2B8n0l7r8oau4En3g8N%2F9AQAZlQjebTPX6976rIkfRWwkYfN0YG11OhwNRX8Sxvt%2BxdQFB5W4ckkFCn%2FyzHLPlYuG4bPyw0kn%2B%2FilRaeNlFpRR7IbV2sniOHQX4v8AdRAAha8Cb6dmQuxmxmG%2BIAdQkUs20ad3Are1mZ7HUKP72sRpVb8QD8m2kIpTlja0fMx5gZo4Tpo%2FcqMdmA2KIAwlV%2FNapx7r2KjcEWQkM29NhuRuiMI%2BTk8cGOqUBvXSrsP%2FQpZhYJE3GOVUy%2BCmcRg9fLxGONWMUGUvYPk%2FEi6T2Ds1X%2FSZLoVhIxiYvXGTTfukNZUM82RvkleluUELVjBDimSoBYnntvEx0dRHHJDyTPDrTYhsx6wVkpRuNuEOdJYUy4DjC0baoo5%2BfiHLntXRPVkn39SYiytnJx0TXcdzUC6NvFJ5dOrCdfv8FTY3Tipt8IBMx4QsOCp6gdxr%2Fw6Sv&X-Amz-Signature=537086551e80aa44b36c7314cafc6811389825c34f5c380d4bbf7ed95aea4fff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

