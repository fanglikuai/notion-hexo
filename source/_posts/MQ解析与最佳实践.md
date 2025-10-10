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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVF5FZLX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T150119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJGMEQCIAFtl8Axq7vH1TLcP8WvBt3umu6tT4nM5YYO94LqZbBJAiBPY98jLuzPzO8pDvP4pGWsq2Jvh4Tct9%2Begrxk0MtcgSqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCV7Z99yjNQTnZFxTKtwDKmk%2F6RsQGsEDGSaqd9zce%2FIAUo7UCiZoqGJ6jA5lPoYbxxUKxLhNqNGVob4sFvexf7kVZoZWNqmhY46v1W%2BCNL2bCtFRN6y3g%2FeBoa3lPK0y6niytcv6gHkxYnvm1tBfQs68yZNcrfpUbHJxDYffOH4qy4nNXjqQka0Go%2FLO7G80WHIPjNYw7Jqxkft6IoOiwvPYzPwyeMqZ3cfPbAXCc2CDmMRCPdEtVOjqOum57JS7dO5nk%2Fdhs9QYQUxj91cEmhsdDfUAv3ROqRzl6SlFj6EBSTyVBu5%2FgDW%2FrKQrMQ%2Bu1A8JATnWjrhHsbKKSI%2BdoERBcxh8sFxt5MTq2Ft4rz%2FIS19hJd6a31FSIE6LZDXjIg7xsuc18shjLF3MHkO6h0NR3U3rrqMHRlfHR4GgDigmUo4Xv6fLn1m1CS9QDXs4T8El%2FUFu523z690cmDOB8rt%2BNRzGsp9b7wvNTZ3ln0R9aXYJD4ard45uWf7yO5207tduF%2FO%2B3Mt%2Fpfh%2FQXbGQIACSoyx3lcdENvCJ%2F4z%2BJ3SFONHjghitqYbtoDI%2BkNMTGacTXsOCmmhxcSOJA0BHVMqo8%2FnNHq83GcpWHnsK%2FJe8GL0%2FDovQUY8yQMpRaaEywzLh2ZwQomh8GUwvaakxwY6pgHJwWhMFata47EpTpss3rV4rDgtjUxMtz7BpAr8X%2BLIw1k982AstW1CqLY6qwM3P3vrv3xv0H5B72Z%2BFtq8s3RFUwjhqHrTuCQGprvbiZ6%2FKbbvDJNN52YrNta6zelml9GeKOmFBq1g3TwVtBOHynTdyOsphJjVNyjn8%2FU2ve7RBGYIEEIkNwi7NvSUHtbWyEofasjvPfHpN%2FTA32wMwfZFaCup95Gg&X-Amz-Signature=185f663c806dac3748f9befa405c6a5facd947df0f5630de24f60a0c745aa584&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

