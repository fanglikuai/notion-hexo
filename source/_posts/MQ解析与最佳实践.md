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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6WBGXAB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T030108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQCyj32h%2BacCdU2JLLnnqf40RTIh%2FPIuJ%2BkNd%2Br3cYn6UAIgT2KQbcegNmHbUtLTx1Ocb214tRcZuag51DDr3e%2B4tlcqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIr%2BfmYuyU1wZako1ircA2152UWJn%2FUmrIunjm6er2A%2Bq0L4kJb6EwKDMLeeDvVyy4l301qhnF0zzTh5AoA59pu9%2Fg52Jbi83YEVAoFODsjJ3Fo%2FhqgCsKufSeux46fRrPfshiJz1TI%2BlrvULDKbxtX4oCzb7tr9490Rwt3Ll%2FD%2FmLiyJjCdsL6rcsoFac28xLESYBb%2FxHYAM0eP1hlceQMpPqoI7VpW2R5Sb%2FwLP4BGGS5trFjValy3OiMh8hW%2BnpIFLfyFRkaT2QcnbC9Eck%2BcjnanEA1GPX%2F8GUwZfzrxf3SKG3CUMMOIrnmr4bDL4wok3EJFExcOELri3NfDANkfGNOAbAtD9Xfljs5jM2DptMJTSqzlLo8SAcwmtt1Hcl487VEFR8stZU2WJP9iIzbqqaaj3fGAWf8cDiIiUD%2BsJdBOO4RNLxo4H239Qsg4Q6lBRq99aCP5KFehsWSAmUdJduoBczUTx8pKsF%2FQgQenOsYiaUhICNyf3FYw8%2FQsr5t29e39Lk7rfNzJUFVqx8fxd8obRkV4Jn%2Ba2k%2F%2FUa50%2FOaqBj0qlVuJ6xbEQR%2B8EJWEZjrQyRmJMGtPBnUl6TW6QGCJFh8Aegv7MqSXjMznsqAjoJNiih1NR2%2FyNx8EOhIUmqKAd01t0qUdMIWgssYGOqUBtMBsxygJpwCbsiIVKh%2BUt6rRbS%2FF4Dl7nMO1WIj%2BGNca%2Filb2gaL0alFWveNUOa0ugAwxuHhV817QInNw6sMEVNvGrqnDkp1WL3gWMia1w%2FqhSdYd8QJeuLPlJysk5z4ROGYCCd7tbkcdJF%2BRnJnLI%2BnP73jxnqjS%2Fe4dN02CN8K%2BJQyhKh6blV8uIibo1enRVTfyhgkDTLUTVRrWGz0nEBc0StR&X-Amz-Signature=f331944b68050992dded84ee70d446255c98debd6f3e4452983412c187984d64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

