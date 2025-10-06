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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7ZHWWDL%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDq6hZFDkDEH4PhCqk4TEOBHMMfZgJ9PYBlCLkBNaRoogIhAPnzkpQOVg6dDIL4ADQxjqzVUziBju0mqtO7pPOT6AkxKogECJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyVSblxF9Cg5gA4aLAq3AMLWTVPcZfnKjVnXz6jxYKTVwabIShOWkIxfdTX8TxNjh42w3JhA%2BK3x5XBxeNbhfTS1%2BLozbKvk7d3mVWNs864HeRjwscNqVXkC%2FdWu5GBuAfPf2yly5f8vXQkWWZnWrz5kbr4%2BcIhePFQoRYV59QofzfxEZ5aLm2d0iu6URAIBJwzfB5hqbO7B59dMIgfZnTzT95kT9NaQftLVFg5CcxlKNeXyfX672EQIoVcWdBgo6FVw82JU5AiIHcNAf%2FLCNxhm3aU%2BAipJYFetxMviW3q8YEi04iJGZ7m0%2FG4Vu4BDCU8B0NrYJ0HE%2FaTw2wuO4mdhRa6kaR56JQLSZZf6daGS4aJAtFqXASSYi8jOwjGNgabYH%2FUmaI3EnB%2BIQKMl8lP%2BzH5stX3pT4JUEevyFG0Ek%2FvkgdX9i9lioHh7S8%2BATgRDKrHHfLXCtFjR1BEgh8J1%2F%2FPMZCCSKlu8vkg2HIemQ88SaeM%2BFR2BmA%2BtuiTPndBYcWeI545EFUgrUQYL5m%2BK0DGH5niubyC7vKrMIih8nexlYhyHEpea%2FysNE0Z2ENEWLNanIq56vlNwJHdb7tM8D%2F0hJS4lpO0BEawMNxaVYGDSg4IVhngB5QVs3B2d0BjUv%2BT0CNosTSxDzC5tY%2FHBjqkATB1ur2DxHtYFaUhSWTjJD4gjDkjVXRiVNyZC%2B9i4Owniqb8F0dklE%2BvDjoxgUD9HjABqoI1QkpOewH%2FDlwG4jTSIPfHwZ3hVIMQ458CM43tc4NHYiWiZYO2rYzvdd57PQy4OgjDn39IRAAIE24Kmq8iqqHXegePQYgamKDIxbvYKr7Ih3cg2pTkWZNiu%2FYW3mSbCSJ4dus%2FsWoFBJgg1w6VBQUr&X-Amz-Signature=4773cceb2f668ea07ea03a68a1d655e37db2ced0ee2863a6da19e189a9e0429f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

