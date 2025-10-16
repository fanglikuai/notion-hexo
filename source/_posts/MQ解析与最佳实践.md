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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SHZPIW4A%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDaVUUV37E878Wlq0NbApguncuH57q7wb3K%2Fj6Gh%2BCXNgIgbOcHQKwBsF1wDAB%2FmBtTkxJGwsWjHe5qasxSTWMVh%2BcqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJb1uJsGsfK0HWYBKircA1a0OR8%2BuQ3K4ig0KhsU9NuKp6gVTViK0WsjdQ%2FT7%2FFp8Abeqbme7BYH0ldRLkGEcz3JPMyUNDWJQ7GpuKGc9REHmqVh1NsGVtUuQefdxLM5vrtOslIJL8xfVK%2B21BYuNjZMKOmkhIsMn6CJTF4rBV8%2FPDFuPyk4xRi4HcL1fWVSZBraYHaPBSMfkcEWsDxoZIS2gFDpjuCis6ZzyMrAdGytAI9OeZLkIIi4jsSqpEmAxLlbTefqnBk%2BOqHRW3xSnITdw5dgIWy%2FVYCeXEHSYrTqPO3gAEE1zJgVENY3V8f3MarZQfCNOVxp5qNnMWxkko8Xls9Uhc7efiMW7A9qWHi7kcivy5BLS%2BFuT%2FAAPcMIzbZH7489E4uy8ZCjft7qu3Km8iS1Mrs5LaxwjMV8TjCv5wMURzn%2BsfMqx7rtIpcXbABOt3ffPduxKD2zy8lxAfvKJEM8JU8g6ohOYCma%2FyJkDKcVWrSM8BkaGldLybph8HubcDSJSCxKJ5xLqsfmuojRHRsvw39sq%2BAwXBqQS2kuI6EBbRHnSgWdUgsqe6FM4uG4DOfnPSOJQkt%2BhRzlJTLQdAQul7OLW2VAbGPHTh9TjQZEi8fwIi8a55cs9c5hIdLUtH0iVIVAmPjFMOuvxMcGOqUBcGowSa44MAuIBQhltJpm%2FH8KEy8U7QPSgsy4rTwodSkMNhmYdc045IQf7jrM1Bm0itxdlVw%2BP%2F%2Ff5h4tmDbKpaGy8Rh4eSHS8ecCj7ogb01u8xWKxCY6%2BJw2AjEKY81i%2Fv78xkGwL8ymhs0SW4RNQ0NFK6Q3LAkRSLv9FVIsBH2jRRFJy2q34uR8ib0eWYX5gv8XMlt8PugD5c26E8GT2l9XpNqv&X-Amz-Signature=6a6e141f793313fd675531724c471883916c58daf06b7c75df29933660cc2575&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

