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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5HT43MK%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbN%2BM9T30f4dq26Z0w04RGUYapALhpuw4DXBIbs1l2WwIgNN9Qq%2FJBGeL%2BYouGt0S3CMomrACsr7yh5qKUJHagLT0qiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB%2FH9ZDzndukDCfosyrcA2aVfoZsLzagkr2PTQpRfwP4aTutJXSPQ%2FYq0rGaFRHGvBaP2B6RhZzakM9jRRanEu%2FoLJGe0QKlx4uMcNktmu0xEsocIqIVKlr65ZMEzHDDj3qywJiJfYh9TFTTUWYvzJFw4XMO2uDPLwKxRUhF%2FSflfiCkXkcwjS1AkD4GFTeIRwoSQml1MuqmXEl%2FOGx6iOot8vcvcumo8r5UHFpOacPo9%2BHMmTbFdCy720pHkgp7EPEh29hPJDjqSpuHJLIjomcBAOYISuODobv1l3BDAM1OLtqyXADSQ4%2F%2B7yfWjvXpExRU%2BABavcG23qPHPJc3mYHX%2FrSw8XWhOWJZOShMx5kUxdUquNLOYMvVayzlbj7bQixLyK9IpZQh4fwq%2F0PA2gcOJUgM2KfII2Fzwe9NL27NtljLZi0YMCCx8a5K1sKWn%2BbfcvLF%2F0B9UE03aVFhUdtQjRJSsUSw6xXwL6klBVOogtnKNvJow9ct%2FGL09HhlcMhwIlS4NT9yGLWZFhG9cled2y5NUGMhisZ4UP9YqLvabaZFxFkjk3Y7%2BIO1I2zdbo3bwfIswT%2BIcpbYmKRhbwgq%2FKFx8pFgNkwj4%2F9eeYu6b8oxFa5jYrl4sacrXaMKKtprnRt46NAlNo%2F1MJ2gscgGOqUB93xLuemZOypfxfGJT93eG0rwqVS1aAedTgAp6c%2FZHFkTKXXogv%2F2y%2B0POTaVegm%2F373XDFMdcXWXsXiK2LeJ2Qhp9PDAUzYDsz8iNHl3Sn%2B0DEgHfGJQSGz3jVDsFx0lSz6NrLDo06e5qHI1VEhu%2BqVgm2oUS1GnVLkALzecJRLGd769dfrz9PQJmR5OAxeYYQ7tVgXS3pEh5VrxBbriNtdAhVtp&X-Amz-Signature=5fa502ac834502e9c8c8faeeaeab327eb3d4b6113bfcdb3ab75d3d023a3482c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

