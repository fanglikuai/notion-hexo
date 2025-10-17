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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J4FDKHB%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T180056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIGj%2F3w9t%2F5nMieRMOILcj7c%2Fs5UjUFYmS18AB1%2FuiwI1AiA44xyBkeBkX0IaRBMeuVHZC6qM21HYbydCrk%2FI32gLXiqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FLOc%2BYsiUTURiUoBKtwD5j1OuE5dtwBSZZKOtwkTS8nbQCQIP9AJNxA7WsTqtxgKva7n2QbFwHv8oZkq0gYPZWf19LhMfKDuL69qvS9WWbXP80bcaK30feFRlc%2FT9djg7OJyYmWR%2FYFUqSA7qs7rQXS%2FEvcVwOePqFk9Sk6qBBsXmYWq2E00O9B1njCAQrvvc%2BA2npsHFsJgvc8V3lTWUzJO1bG8qsCDuxHaqcYOGdTmJMny9mgqAEakpPIaWAXG3Jkng9FjBfBRd1Tt1TmkagXEKcxNZG8JymrEE0HI04a3tzGRwnIR8kwBjQCs%2BNinUNPXlT1xFhoGv8LFurAjgW6dDS0d7e5c9Qvxu2n1e1qIPhnuD%2BLrBwb4K%2FsqzBCkzKj2%2BbH%2B%2FUMoKOPGGtjErO5huVvpScsRurDIpMNpLlzm9C8%2Bxng37siAmBdVg1OouYWl8M24vwe6B1z3wWWRxAI%2Bjw32JHtZKFmUglAhNcGzXnZoGBPtaPbBHysASTZ1vSROtarJ3h63JQF7HcUv3QvJdhozYgywqmV%2FWnPkgIji9XYh%2F1%2BIT%2BjAsK2Qqb90Fai4XoPERSQoKVYeHHt0lsws5iwWcaciYPytwgehMkaPxWfC6bVLlU6vaMkLtciKYDFCd05TYu8s%2B48wyfzJxwY6pgEXcbSHy56bxLCpbrCxhLtES5bxlx6VWawDMdS5X6nMtK9NHQS9kVxE2j4rbdfM7iUIWVaW5%2BGjrWQ%2FIhAXdyg48HD3XR75LC8smY%2FLFJj3haYB7G0jJ6WzHLpFeQaC53QwaR9V22p9f68ysp6Y5xfHz0ZiqFnwh5koHjStGbuTGNGPAxFrt5ykSKTBkX7o1t59mnOONef5l197WpHH8Yx8dmQ2gQGW&X-Amz-Signature=6763235762a2fef0afda30b35f23bba355928d4c627c9be471d912bbccb61d21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

