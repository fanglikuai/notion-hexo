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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTPUKETF%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJGMEQCICXVA%2B2ZSXbrUYz0u0rByrqljINFJ%2FZrUNdO74e6dj%2FBAiA2f%2FaNUJ0aVqPPw0RMJhuKPAIqdnd6qU32xP%2FAB4TwWCqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHUIc5okV8%2BUsdjnLKtwDi4qTkWb6AwbxK805ibqUcttC4o9UgceMXKfwshrpWaPT%2FmN150p%2FnXGY92gTJETXJuLA65jRc70hRSk5M4sMdGdZ2WwdmNm8aq4QLlH%2BKF%2FH1feforNwcL4UjSZEGcjgSvbJ4o5oC05g8wXPK2%2FGcNEgIsQnfGpmskoxT1KL%2BzJmd7hfJ%2BmESP0aU1FooaoyjglDJ2y%2FWHsecaesw8vckQ7bP7WBc7G48wb1gpwjdOfkuID0r9w1AUY%2B0L3Kl%2FWlzOPI5WJyFl4kgKddFGTDDWSdNLjGERfqiIZFK02bAdQCLGfB3mA6vJF4V3w%2Fp6xN9HQtg%2FfD4eQTpL7u2o%2FauTqn25wp%2B4oZIK1pLr3fYVeSkgYiWzlKTLLb9SyfEwFWDUcz%2BD5prznWJJbEZSa52MDYhD2gieUaCcdIL3holnCV2IWkP7W69OGwrj3kWaJc8YD4h0fkkP2Op%2FWG0Si7KJYEVB8nRMyv37GIzCj9mXszN3ZwzPO4UB5EcdTuPV6zZJEDUCPVvPCPTE1EwA4zjVfDNxpizvZSOpV2JzH57xIUnoa3ppS%2FBV0D7e3%2FPncOKd4j0iOmqKwNnQTMzFgGzZtE03If52g%2FWIBXks9LIKXkQS0B%2BdPtgae3Aqwwt8jsxgY6pgF5ykxfuI9U1JehHWoqHsEzxlXUI4ooSsNGCLFQ1%2FMz6LR9KIlzqMzIVbqCRsPZqT%2F7jqahwX8bfD2%2BF6rTVBiQlBryjVmTD4GbHwFfpBVeC0SI7NNBarfWNyAQOvZMkbnMdAgAHWQVnssctPRPCWYL%2BwdfLY%2FUyvZmfV%2BN2Wlza4tZXpE2jrD5FVnfgil%2BDbMBfHBuHL7aw%2FV4dy%2B5J7i4J0L7kqy2&X-Amz-Signature=b33cbce3c5d0a23c224efa5552b8d3ebd69abc5ec428a7aca9df7489d0c4f9a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

