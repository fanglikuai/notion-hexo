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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V76YQ4M5%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEMBaONppG%2FPxJvoumXemSgBFjJJ7gIGmIIhH4NSII69AiEA%2Bm3mwU6Oe7Mlf8XRNiqeETF7J5dNOMeCLkl%2FV9dDmqoq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDKoPzYz2ol14n9%2FO7ircAz8VrUXAA8%2FCgt4TckUWdJ8wdl3KV18bc4EbZw483uykzGbZz6q%2FJ7UtIfEVTdQYellcoDRMmcFw%2FYjwgMwFqNKg3cYy8FjovExWtfNhn1CvA4ZNJYy9S5etOXmJ8BwJ5gsOs1sjQJGBlqxVFjMnExqIXR298E8K2IJKRByT65BE%2FzMvKvawKdFAJi%2BTV%2BwEk8JSzxQ%2Ff40%2FLwJK2tmAhZL43QMkL4XqSoWLa1su8oZ7R7Nd2MFpOMyTFI%2BWHxsYolbuBPPKWwAuZVAb6IOioIH6aDE8%2B%2FBOjUlkLpZA3jgYVkqVW5znwGc5fE3OqH3r2cfHAX9EA8JbcLdihIJQJsfxV1KJIkLaMeDUqNBxFIvMVovbBwW5JSJnd79n8cb95RkjM7Grgl1da8I6RNSjxIK2ziXbmR2WxKoaZ3QV%2BWjk4TvK9dkSsie8qGY44Q7yeqd6Mug%2FFVc6UHZ%2F5qx4v%2FuwWuRbP%2F7lCCfUG06YxN0AZv9REAO3ghQ2brV3iuLOQNoEAIIzPix2K5Anb0fCXgIulHHE38lWCAp2otK%2BUh7vCHIKyQshh5nZRBUO7PFdLhWvkD3lHCFv0ILjTBa2K47D1mXylRwovHkebtZ9gbvSN4huUMPT3lsGh2SGMO7owMYGOqUB57ZkNgckkbJNFoksFaD1sSKfv7jW%2B3XLoDbCLIwhd6flGfxHFyLHI69kRLFf%2BkDG%2B9KXeC5fkufGP%2Flr9gHyVnhrVEYO4%2F21yBMH9VV4zFkTPx7e6V2SP%2FOwsBEPsnNLUNpd2iYvWJgkI49%2F3o9daxUYul04%2BKfdg58SZNF8CO%2FYYj6tFGHHZ0h9IOhkSHWwdEMLvjOqpj00INJHtI9DxCzYATyt&X-Amz-Signature=9e17f019b393a41f02728de8c8c783d3f0b706b1a23f437173e80c42ccaa5185&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

