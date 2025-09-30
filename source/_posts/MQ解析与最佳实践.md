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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663MRH375V%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T160105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJGMEQCICxJJkA4jAbZJxOMr63ad35pZlHo0eph9bkub3ErqxghAiASxzNOrMJTSGojHx0CNmwCfx2Rwc7PN71puUutb%2BHCUSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMX7WErT2%2BMn7H3kzQKtwD6mkJ4yLEFAabILonvHSdnQXkwQOtMwE9AwEedL8Uu8OVjs%2BFffK%2BMrD5WFBANTsJniOzVthljFzXf1YM82RxaLGiyCnMRV6VGwi99u76G3yuIZVrQszmniXlkzoNv22y9H53R61lnYsUCgw%2B6m95X7l5kvVWOeefB42PDuaSvh%2BMCorwUCHMR9spETWesICfPiyVvJkaF2PVig8Bev%2FKo%2BFmnZfDZV7uQ1plFsdHJqPMoGhhaLjWYHoaC7rA%2FtLe%2B6I1REfbKRe21LrHRgMoGmhCjIZgEefX1Ty2fLZUJ0L3CYyVuZ%2Bmhe965mQ67P2lIVKh4gB8VAmfJD1uDtZGmWsidMN25PtIZ70SH5CF7v2QJuJsXcM41wCDeWDKPYK%2Bom9BaaEifi0dyKOzWjwDEdvrgx8yMYThFn%2FpYhFsMjFz4M90qsY7DyU6F4okUdHHHsVcZF0GKIqQyU%2FRDLaJfXdalrTrmUQ4535UjPgokw6q%2BeIkEQbj7R4KKIveUH3LuP9qtiJowYmiOiMlS3g7cFQxY%2BvObGZMZslgTa6T%2FQLH%2ByMvml%2FyqZAqCDw9jwnFZDTB%2F4x5YzYBmOTpRIUhRVeAKkVIZLDjj2SvDgtlDAmEvFM%2BnPb1LU6q7NwwnvrvxgY6pgE1mGQabyBBn53og29iDcoaIkmu%2BVwsXhM18HBMcGBSnptCiIS%2BUW0ErmBUb%2FLQ%2F%2B4V%2FraqNm8qFOhPzfEO7YzaNAV9qPXKpJlbkDI8IsaXhyZVyLGIXyFWxAiGlacYZmWx%2BaWfZ8TFEW2CLZFyNZFuUFgWKWAlLp4QwOvf%2FzAUqoHtjAqgwRj%2FSUKoOOj4LeON4GeWK1OlgPcQ5y6jEn9%2BA26BtEYQ&X-Amz-Signature=4756219aa8e132b22ff7dda27a1322ff69beabe1c59f6f518cbeb89b7d0b3b39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

