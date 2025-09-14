---
categories: 整理输出
tags: []
sticky: ''
description: ''
permalink: ''
title: MQ解析与最佳实践
date: '2025-09-04 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIOXRCQZ%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T132611Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHi3lkeA1OZq5h4c7FJF6FEMy7yb878OuN%2BlvUOnnXQ2AiEA0TvrnB8pKNUptx%2Fl3mfkj1HaWqTLj8fzJQ15ybZ4Iosq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDOuxY8Y2MxhTm0%2BEYyrcA%2BPhqJ8KrEpYxi%2BWUoUBSj8hx%2FzmaqGWxyNDb4iIyf1rkNdS%2FjrXkJvJQIjxQdNwG5z5BoVQMZHH3xVcKCG9Z%2BDUOTjSv5ONmHAoLZfc2m%2FG1vWUFHAZRFSZtzzDX3dRSBcWPuKgHafapHeACr1%2BTnnKIxpiJ2rRdP8ugEagNfJ4oRps3p6X2RUXXtFfEf%2Fq93KsA4KtqIRju%2Bap1CRIrFb9WCoMUjdqChNI5u4UXVPLSz9mpUOp5GFN%2BrI4lkcg18G%2FEhNody2zagjm4j1z8VamYSmlPiUq8VTdAzxngdH8Aoxt6OdgQWlONcFoFDSZ2BTcX%2BGm4kx1WIsnonGwSgfDBgLxqryqrcq8cGFE4CLMX7HZzbL%2BnpCgAILv588i8WK5AuWwqAVfY5XxocRUqoy3NY3PRMEwxgDlo%2B6Rx0QMe5HpOREpTbBTfdNmSX10wuNJBDPGEdbkl%2ByDXnYhatrZxwuwsLS7VP2LtPFm8sS3zP11Ke8A4AjmqYWHXJMxWUZvJSX2sJo6a%2FcZw9bhIfN%2BE0epL76BUdTvATaaSY%2Fw71Hpxn9rN%2FjapRKX0nW3qZwdwM8XNYjzkZ%2BW37q2C2D75FAQPWe3lwLgZdESGmEagCDKjV6qr%2BsFNH1YMJTsmsYGOqUB570zK%2FXIA1Evma3kmBYSzBAiR1hRX6jtbk8szCxI0WE80fgd1DhJE3yCDvmuzslkg3jCR2o2%2FEJ9nuFd%2BGqCBmBFKN2eHRDJpwpso2DE9Kb92FDRVmUUjCkh5fvNpAaPKj%2BufgN1jfXxNzN2bBWD3sMg7wMe5l9655uczzjN0Eva%2BEV1%2Bq41ZmpDpgA%2Bqg3nSq37mpQX0X5qo0Nct4FWYQgp0V49&X-Amz-Signature=4cc54ef772d41ea7bac78e5175d1f399b8b55a1ac889a15fb554a53210eab7d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:24:00'
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

