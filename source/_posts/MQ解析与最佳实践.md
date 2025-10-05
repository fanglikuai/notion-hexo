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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRZE4ZMW%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGK9lfW8odUXyIlVUzB9XVmMKIavzMqtMlWiZGr0uevTAiEAn1iB3rPKAnCHWpTxCfLB2sbNCocYSVfXPEhwYs9B%2FlYq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDDFu8GnUmsp9Y5bxgSrcA%2Fvh%2BXOul%2BtvnjO4vasfrSOJgtgRKL74RiPh81p5YUTbCXK1vtMEnz4bJyFOjSiRPd2YoKpvbZfHigIhiEm0nY%2Bfi%2BbDOONGK3G5Q2rKcgPDr9FdcRreZSjxaQbl685ufPfcv%2B7uiBmAb4muj9Ory4OZwH6Z03smLEJshdj3T49nM50OpbUv3QSDlpOAoN6c8sR7TWyRVLLVN7nq1kHAnRrjblQLUENDi6DmdlNjxCdOgV5l0w3ApKgAUVeReK%2BTjHythUOr6hJptxFK46br3xb1p5Jr83lM1BqSXOK4HvEVNAbm6cbR4DPjEkVUMTHk0HL259ptwO1pFNPRLluWyOnWikT%2BWL7HD8V8kH9j9%2FQfz1DeYpir0xhsazZv7taaAb0fTJpl9rN06j6XBEjbhyHD69kIdAl6VBkbWAhtFb68El2jEKf091VYzWf3c85Sq778bxOoK1dFlZlb9FM%2Br%2F8i6elHI0fFrY7c8dGDA05PCzR8z21cNDiUIhC0xqPQWFumLl4ZmDYVk4jK%2FKRQqVo8ld7GK2nloUKeiMGA8HhI8mrNyiBWvepIxkmQ2rlkUbOfVgf7awravQs2UIeyJNouw2MEE5dOpNxHT1Wo1vamKBO8Fv%2B9creyhojgMNLhhscGOqUBWNe6uWYeZ2oyWZALl21R5Lers7%2FMAPBgqoOh5Fo8Cu77weoRPOs%2BvL0QusdTiX4VWEbepvVFbUNOEAfx1TUSlxFLrHfHFwuwsMGCxgwMTlSfFzdsbMAp3HN%2BNN9xJn2yVkjEJUYx6ZuToWds5nFTeP4zDbxJYsId9o9J%2FBGBwCWYdM2BxohSbpVQVFl1HOfBkCL0lK3XWBAN8zcTe9g%2BK87tRpaH&X-Amz-Signature=e718534c275a369c6c95cb75ba2f62f2377cfc354e2518b37839f9294320049c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

