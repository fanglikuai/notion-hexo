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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667O5SHYQH%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJIMEYCIQC1Gdomf5Yhvzwu1xRJ01Ip%2F55RV6iJbBuF4kv8XgoDEwIhAPsT%2FG467da3gIYVf%2BWg6MVGTZNHCnLW0lFm%2Fs2o2qdeKogECND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwkAsHJuZ%2FEsAuXKKAq3AMw2mv%2FM12nrQgU9u75fNRxkkFnDQapN2XB0Xqgq0agImDfZ%2Bc7%2F8T0iZEChwHyhn%2BzuEwv%2BhgWa2mzbXOv6yP02D4qKsvadPbhc%2BzT4LC9zJq9Xwaa8J%2FqSv%2BlYkRFtNtqhpjp%2FZ5h59pMy9hpgOMfIXRp3Go1CNg5ig4Sds83gBuwLiCe3Xs47fnrqMjboletExDregBDiIam49mwRg56HmBbea01b0zp6Omqaiy6GEcQuTxmnnj3%2Bf%2BCF5QcB09Jsjs6UHjawVteZ7ye0J0l5dYm0dY%2Beb3C2uKSw5e%2FFZsFzVHZXLJ%2F7MN1iBbmvhtyPrZvb2PPsrzqWEKvuo2NZVyk4xHIJJ6KmW%2FTCzOVNoQMXVppW5rSJYuAXqZnLF0%2FrqWT83AAgUcXPA2lo50t4daXyhHS9Po%2BQKs2yT4327f1MTblPKxf45joSAQKmCYdTbMylomMEUSqsaiV0HqX8IO9ieokPHojdConGB1mHJfUY4ILP4JjO8Oxp%2FP3CIZ4Ibou8z9lumvqkpaWBqZEE7W7xKHeP8MdCfgppE3khoAZzTOlSiN6h1BeFv3T1%2FmhkRGA9t6r2SI2BUcjD7ehZ%2FGJ17Iv0hU4g0c2PBiUXDmNtN7aAqkXZMYp%2BTCy%2B%2FPIBjqkAXCDJRh6RJ%2F4T8%2B40KN8%2BWpezrHdYMxlh0NcM8zProWJCOSu%2FBYo61aIJKGntqS3FsyUGN7yJ52WJmlcl38JqQNt4oI6S2JT5P%2BOZZjDmaJ0tbcR2crc%2FKEJsBUJxgypLh6VJ%2BvMfFJQbYM%2Bq0FYtodenP%2B0p4bIgIeXq2mB33WFl2NqZizVJJVpvvY81I6luAmAcRK5t2amPmCCVvqB5XZbqEUx&X-Amz-Signature=1b260b1a6791c6261b9ebb0d1af9fba2d05e73e9044afa53437d82581f3f258d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

