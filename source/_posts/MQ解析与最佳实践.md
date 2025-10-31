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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLFYAIAI%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJIMEYCIQD9MEye1%2BjInCnqzWpInA1z14rNneoQMRN5LrH3rWfyFgIhAIN2qFFQLLZ%2F3QYEkWR84O3u9D9MVKyzIfCkLj%2B7G8jJKv8DCBkQABoMNjM3NDIzMTgzODA1IgyG24G%2B%2FLy%2BKht4h70q3ANu48FpbJKTizUhNp9MeE1RE71ZcXwRIJWp0l%2FcNpIZousbbT%2FN9gTkIeBLN8pQKPCWzf0ZEsSd7vtOQkKz3ErGzfgAlmUBK6cMOfFSodiotW9a4poLHk5Jk5XmjD30iD93VB6S3OOJxI076ALjhFmNSUmBLXIoxWRK6PxtkaRCnPcYFz0VhYZUFZ4pYCNf%2BB%2Fsb4eaiXWjMhMT8b4MkBvQDDh9lofGZGlviEHDzZLKvfTeptvKmqpMxHJ%2BelseFLD%2BFMg0%2FqvQRUMuXEUvqsnIPw4994rMvLU6v6DKW7YSZbtATrGmJpNaJCf0DbrGYjmK%2BvlJnNN%2FVeUODGCUHn%2BnGRxXCvioIOi%2FOJYI22IPj6JNW8k4D76PuFAlc6iWzmvMgxjMC%2BZyT97Q3hR0kyCvBgr%2FJKRPwjJUKO6yHK%2B45mSUTYR0fsabgy43NZs8EEv4X2hY5il5%2Bd3baVpTGTnBt2fhRojwkSe0b9kMZcXK5ao2n5MlwSJWfAaV92ing3%2Bg5xIs0liUnppy1NyJATxoMgnUPyvYJr2QVL1UKd3%2FcbLs2537PxFVRQH0HG4gPx9cDekONLAxU3GrciqH7yVuAjm%2FvYqf7kW44nfsbPkY0liwOAXvpwTSabK%2F7zD%2BuZPIBjqkAZky6Z0q18w9%2FlvAAJyBnVs%2FrwkZt08td8d55oSAzgATR48HYPK7Hzk%2Fl1Cs5Y9VdUCyKvhCbLQBYD8N6JQYvAwhT01LvhLSZYBB1knH2uoEwiVN6ZWk%2BU9L0ph0hutvm7H9tQZVLx4eUizWMM2zEfhPraW8lqjZVxJEcJ5bNdhLfymgvm7X0NZznzzeihgh8o0Ze893vg0yFX%2BWPCxcNBefmE%2BZ&X-Amz-Signature=29a7db1e2f5556647940739771806a7632d2dd3b6d5e1e5fa7dd9fe2a3b5b228&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

