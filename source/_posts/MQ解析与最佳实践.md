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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S7KYLXRA%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T150106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFJ775pVeEzvNCcYiXKAQPMU6tBICbDg7CH0gtvssYF2AiB9aR3p3qnuXaqWhJmWnW4O3MjfMW3rVZ54PQmqde7wjiqIBAin%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMtM8IlNkhBSsw%2FflEKtwD88ZtzNlrO5EWkk1iTy3xPDmD0OGQP3xBPfybNoPUrBiRuYwQnP9HLo2w0pI%2BjzBzf8iQPuwwgr7ZZep7dFiNeg4mgL9LlfH3u0OMmU%2Bt85KmEI1i4la1YR4KrEEoFDU9FJ2NkhlG80YoZwBr5l6kF7CixF740qUu5jIdNLcRKoRGlldesj4xJ5k3F7FjGd98zAkl%2BZKIHhtZZAOa7%2FjP04REwAG177p%2BALgiEHO%2Fw6YV6VgSZinrxk6KRITU0ClWetBzzAGHjxzMXedNxzSOVvAyKKND5Z5K6Mbd2ojeOID0vB9S%2B2NWNMMMqJ6xVdTGdxEFEhEtAyAuDqX0BUkEWMzHI1HXBmujnam8pBIQNi3F%2BAvrsE9%2B5LTAC0ee75M76Bfjn0a6fUVA2oaa2V1PdN2ejgYb%2BSe6oHjKOArFKlGzPacIKncHaRGhMYVery98y0CgEpYXf%2FjL8NNUGsLmfFaBGYlledm6P8Wf6LFtmTwX3UuOJExqusXeDpJjmS3RJTQzBsJG3YWX2SiTNp%2FYizTPHMf%2BIwlJymj4kJg1TgOA59NE%2B%2Bhxd06H2QgXZDj2zS%2F55cEbjjQ7nGAkb7tgHKqqOzG8NtkG3KjEXBMRoXB8LfTuJWU6nZRX%2B8ow4%2F79xwY6pgGdBqKTtIym2OVErcwV5GSQ82YHiwbBtH7K4KIiLwx0qq6cSrzdR1M3ZbOW25WisTRVaVIxHZSISe4kEzBrJDO0upDFZgqwGnsYAJBvlxn%2B6mlJld6dJNQ%2BxgaPdaAAl76JeiP7afgY%2FSxyooOoCCLGh4zVoMXUHro8OR0duV%2F45oq8e5vphS%2B17joqSHFEyi9%2FDQdUL6EBAr4TtDa%2Bqgl1wtLixqQ%2F&X-Amz-Signature=1a6a3f0dc0303947fe57ce5a350c031deec548b4701e312386731ef344b3ddce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

