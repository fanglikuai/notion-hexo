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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGZRUSCD%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T220113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICcyaYecxAEBNnyBy738c0Je4e8rQ%2BrbOe2IXEkvmexvAiEAv3GJecZH4C%2Bbz8zP42aaIhkUFPJCMjewi60Ua8G5Obwq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDDCsqYNVvafZxLkRDSrcA2vBaxZFaoHa4bWAUOFAfe%2BE94CjJy8oy6UATHxD15TFiCs1hG%2FkDyhFMGxQOEk1KBSlTMdiyQyiZ6A0wMClRSW8zjVCK1GygXjLrciOe68LO9L2Fx09vmmWenmjf1B2HqBaUziPJsSkZiSFHHCRyjMuSsRRPl2MCRu1Yqw4W1LfNScn5RBdk91sjoLzSpIoR%2FWIwmkjleqYXLNSRvEqbasp9KsqIc6fZ0acBgBLkNIH7MJbppy9SkUFRvQIFZ2B2qhCHDYy4hYJNVMqGzgMI1LYk11MiNXGwvJyJ9pGyE5RGH3rlcxzMKjX1uq7fMDQFHA88OWT5cIWrkQ%2BpReLXNTyX%2BtlaeElG9neo%2BRh3cq0G0C4%2BBlNFWTgTk9CtroNW2DjUW%2Br7BxkZ5fysoCEzJG6R%2FLUy20u50R8PgLjLpiTEmnKURMpi%2BtrGSwnPQIjy4r3T7LruvPZAHD9qGOdlzSiyiGG39Y1SMSGD6B%2FdlkgqzD0PnR1YR3br8IHX1xdUnnBIGx9NY%2FQUA3jft%2Fd2riHMHCrL7kufOjd7pa6uXS%2BPdYoEbkucxfMvjW7RXhaZLElKS18Y4kTLWql715Wt1iuTNeyTqBF1fVDkXamIt%2FMIHPU%2Bi47bIzOxoO%2BMLuUk8kGOqUB9fNc8ZXZjQMFEdXb2hYcWxCpQVNtH7pAVYxBdwi77kyRp0oDS0%2Fj6CtgLepCPnKOZMyPf5XHS8ZIt7AJxJfPODRfITNXfS4jPsxK97HwB9dCqmlfymusThjGzzyAYU0C41mNfIOZZ6EijY%2FdzdIJ0CbzfBABQAZAJJk8ppjcqsRWqpXxg6UbnxEvbqvd8y5CrqEFs5GFQp02eokrXIuW5j1FKA17&X-Amz-Signature=1e963821e65feea9733292831bd60e99f7e494aa0954e8a137effea7da1e181b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

