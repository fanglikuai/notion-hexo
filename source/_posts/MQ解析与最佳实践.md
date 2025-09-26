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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTT66ZS6%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJIMEYCIQC3%2Bzc61VGGey1aLSyP62uU2uJZ1h2iLjLFQLLd21F85wIhAIxGoPMJHc6chQuSyZpby5z5mkrYL5p9mjZfH5jD8oefKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzhcUc7W33TpSfSXJEq3AMpPNj33%2FcrAd0qmTczRK1yKC6Y5C1OJsxnqNrRxDVyYPuI09f%2B4dkxtP0qq3KfVvhJQUgFYIFoZQKB1tTbQ7%2BgOpoOW3hFZk5QYwMJqITrIioTZhGNKwDT6PHDPmWWi6NaGR58PyJ%2BUNAcZj2ArEmp2Y80viRHa1ymTgZIThWCwG8JLHazuOA5I6%2F7%2BBznuGvorfAwA5dOqWCuN6lVA3schfPlP6Am8y%2Bcca2kgmblzcq4CDp%2FZhVvmYLU7dJbPsjKWUfDvHKfWMZDcL44N5jhVw4cpNzfl9Mjw3nBPrmNJZytBhu4EJfs08fMvYCC%2BS84Yq1ILkqVQzPpTlfJLTL%2FTdGRbcPj2P6tVvFj0RpgVVQk1eGvhZ9zOKMgcT2gc6dgfSN6A0C1lW7HgUdXT%2FvGIXl%2Bp48RSKacKMj7j0rtLBUPnsPoYXUVP%2BwsTdhAGBiTOD%2F5OfDT5G40g9yyNLTEUklEfHkF3snbB7WC6mvnJpM93NRm8SAU%2FvA5LrjYk8vvPs0drtLsI63e%2FwCiG2MG%2BQqViRe4jjHWiHLCLC%2B%2B8fzlWKqzkZGKFoLBsK%2FQxUKhLv9p8CeXGtI6UYIl5KmQGW0tTXs4q8pM7JTDRh35DwUW0NvM5edw2efGLDDclNzGBjqkAZyptRPqsC3jiAo4Bd%2BmnrIeU1W%2B2VqiyzvO3kpvCrruO%2F1HOXceiOKrZI7P8sr8EKcsR9jXIpLvS6%2FsN%2BXDiryvM5G%2BnuKqHM%2Bl1cQxBDlqGIwF0m3FtRqBylZ%2BWKLJMLi3J%2Fh6REc0lLNMr6Aq6o4AEBmWI48mPlPDXvtg6HDISTsBNcjzXJRVPSOPMCTnnUtH8pNdIUwPnI8mBIosxXMZM5z2&X-Amz-Signature=4709257052593cb9644407215d5d3fd5e0833436169435a9e2e047c8c86b72e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

