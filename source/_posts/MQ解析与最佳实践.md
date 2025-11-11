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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DN4334C%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQCe8uOkl9w0eBbCW62QUilxsj%2FwoJwTGYzvTpPLb5OXRgIhAMfhbowZP3u3AX%2F9XS%2FdPQCfdgWFhrC3av4f%2BK0GGQdaKv8DCCIQABoMNjM3NDIzMTgzODA1Igy2T4yUZbyfzeSTIwkq3AOA%2FOGmqvuwJk0QIKo0iXvN%2F7eaeW9KrcjSL8XgJT5458zgpCM55gR8Lgwas%2B3C2eOoYocaAVy148011RZA6V43vUwYyphYBWmRp1plDUTe5zqvhUqlmrDeu0MxiroCF4MslgU05ipyUwmDJjuHIyhvwOQZEXJXJxS5Owc%2BfUxu9WmQ2v7qdENt1KO5p7Vt8xCI4i2fYcLPy0VJB%2Fp3qRx%2BS4ssaBJrUD4N49VhAPTpTj8NZVSWIzOUfpOs4O8pd7pvwgYUcyYvhOErZy6YSTRunBYRl24rO7AD%2FsmuZe5Myc6bi5eUrOg6mQwOKKSdKOHC4sHOKZw5bzBxyrPXvz1Ejd1cskh3OFm9DXCoZMWMKmKPqYdOIyhqltA8YnMPqzYTa6qtMJBNBbcv8yFc2LweqBSHchoYmUR6N6RFLHDlWhAtz%2BVI4uQI3IgReUeYJi%2Ba6qzqSqpHrVWemn9%2FUKPSf%2BdcTGt1RRbgMwnamgCkShn23h9Y2HiooRWBH1xnjMx%2FYO20S7PuzwFOmQoty%2FK5ApJk9wlJfMAuSJpGHiWcA4yucenW4dNKtdGH6ZrVgcLCjYK0oYvko7vaDmKQINftBLC9%2BUWQaaLXlhNzblnXQo7OR8MQ5BlA%2B%2BWXMzDp383IBjqkAa2hxSmzJWiAhMgCEsiKqxET1E9VBGgYZgmX2gI7vQWDjoa6REOul5SR6SSFxB9UidiXx8g%2FxfYT8ZPFNLUyjLvH2jTXvgqm6uxYvxxVdUB29N7JI3EUNWkQQNlRDTvMqsqACe9YmjFLh1F7j9S6261XYChLlE0KhAp270fZztwjw962bjzFqo4pICyvz6%2FDOX7zYWt3NLixATWaTCCkQWywvoFI&X-Amz-Signature=8c50dcf0f2ec3b23f58dad417f4e9aa2e4e3bee2175d790050584ca694485728&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

