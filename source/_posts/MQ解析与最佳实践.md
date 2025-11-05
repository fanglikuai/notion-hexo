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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIFVLTBD%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T220041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRM2FZK7moBQJXSwX2%2BTp4IfPDkig3dm8trRtMhWyMwAiAGmJoxJ9R%2B1%2FG8suTw6Lu1SxiyVm67enoTLm0kFO56yyqIBAiX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMKH5IAgDjKsa3kXr2KtwDaQQLh1RzUlIz7nU8gtGZm%2BNzt89gPTA8GxPhII0q6CpirCRsNl%2BCgNz1nDFrKAwnZT6YHEIPpOr42BXlroFV1Hha2NOjL67RowjxVb60NFkIogwKEsxmhJ7Q3kDeCMa0629BOCcULxz3eomvn%2FGvek0LtJTXJwfEY1WG3cjJCJ3aXdfWCvGZ7aS5TS7dhVhR1Bm8HtR%2F08PzWMX7lf49sSIAmBaJP5hqpXYO47zhE5PGqf6rSINNhfFbV3Z2divFsgB9GW6uVIoqnRb5Cxi5lUsLHZf4nkXgeqqW68y3tzarODd%2FT112FIeThZj59PZV5KMWhu9Roe5ndCybgUjAzOfqyVlDbfk98QvNdGRcLg1kRVZJFxFYK7mB%2FLHfiQ%2B60KOlfPKSnzNy%2BMn6HNXkJEdj%2BrRyjOqkiZCdj%2BhQQBE%2Fs1Zylj%2FWGqBy%2FoM6sIiGryJHJFNwnnJ%2B3THyeOyKGBpdH%2FZtxbkMwrwOyRumL1q6v%2BIndap2f6oFFF7nNK%2BNnXDVouxbo1107hQih56jz13cD5rQvs56y%2FmlbmcRXJPhMCGCq06IBQjOwhEmcS4k84cQdrdollGw5yXlkZNZALUJw37XLq3DNr79EwcYq9BR1hhZQ%2FtFn%2B49ZbowpYyvyAY6pgHbZKduwv0cAzqgt5nSu4mmusmg7DLdLnQxRZc8S75cFR4LHECy47xnS0y%2BPzQXN5AwURVnluvV6HQS4lylscDB6sCX8oooarYho5RboX5nonGvhqb1L2km8Qjf2vdUCxLGaCPGlnAEY2dw%2FKf6PI1iXjKL2TSNui6VXmavvGRO4ZwaQ3mqkoZ%2Fze6Zs%2BENYHkc8OqKxcxFUDRMLMMULHa8BK5yub5Q&X-Amz-Signature=ae8cf293709261905ac6555976656810387bbfb8f7da63853ebac0b4c2572bf4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

