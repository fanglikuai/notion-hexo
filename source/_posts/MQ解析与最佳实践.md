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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632JDFGEU%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSxauumrTXY58aat6UudRvqwopD9eIsI4ObwwGtW%2Bs4AIgL%2BnGxgdoiXqQXyc5cHQkwDitjB6l3pu42vE5ThGyvxkq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDAru7WgmOav7mWNPfSrcA%2B5%2BIW%2F6i8IxSYspL7wD85cNvM4FSG1j5xmIKIr2DhtXjVjd62Ftb%2FMSEGewLoy%2BC%2BGLPSN6qWXscQMPEq1z%2FV5lShmQPtRPlbQIfFh9ruh4a%2F6IXJsw2c3NIJP333B6%2FuJ%2BIEWYJRltAsQDV9xcUXKZAhTOP%2BlYujbwfmCGrdlXQivMCh1ZYwWkIDD5S0CSHnQcw4fhvduCW2UKmxaU1ki2SvMDs9j4GQ%2FlpXIioAE0ije1wFgX7VlAaYruqYyVtPcYkcSkpTeYgqlEUMzFkR6%2B%2FGEiaUlzgkT47tU9qxh2XwrX9d2DmarWkKmITn5MqHIhkEzHkMLoedbnY%2Bu4zPKtSQL517P%2F5g2TGqxn0mPuwoPc1fN93aRnn0K9s2ELNZsitqtVnb%2FmXyBnxJKLKcnwksR1pCi1bfN3LU7332fILwuWR1Qbf4e3ZrAZQjXaftdmmCADFBJcI3uH%2BtZ7IOGT%2FTNSqSkDUSQq82cBtaVZu8uN7G6VOeJuPxHxEkSUe5lRqJmot8Yx5B8QaOPJPuQHdLWPJEx3%2BwwICAcEE%2BuzuARuBgWvmn3zXM76hA0r4U%2F48J4YRhfaORetsctoqNL8%2F4P3qH7X90fp7iH6cM0ZfCYMRXtH%2F5NmMV9rMLjRtMcGOqUBIr%2F9eFOT8HWMQfPkHkFyznTFEGdryahpoh7iFAAlNHZ7JIo%2BoagmkZF21tp1eMYICSLUBwklx7ARUpAjn2anf%2B4ZLUSWrU68Sa3ey8d2sqj5F8rYDtvowEeJ1lHrhOpyzsyAY8ssKqsf58nJJEHdi%2FkKIVk5TdcovX6TGDraf6sLiQAnwiYxjNZw9VV5Z%2BMnpMpouYHo44SoTx28Gc1AHAAHQ%2FFn&X-Amz-Signature=1e7546e049ca68e2f60d741bb62da82508649be54ac7786e818620ea6b335060&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

