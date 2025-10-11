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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNVB4ZH6%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDJUVWohvOi0DjeNHoOZcUIK5PsjN%2FQXoWp8ZuOY8ZMOQIhAJSssSlF%2FCHBIvXhgjGCvPiWCyZRsLGhH5dU7A61s6pcKv8DCBYQABoMNjM3NDIzMTgzODA1IgxTK0dfcxariM1ckBIq3AMcgZd4hTFuklGUt1lw7EXXn5FZi8VPYzOwqhGrEe64M6QVypfTIwm3%2FVVgRGzSLKR8vrFIreSQ2UKfJdWBqpGvG%2BQvL7FQZWK48709RuRiU1OBQnTVbQdJNfPBJOvUQGQwZjt16ce4LX6bFKA4C7bnxfnkL%2FvmBdTqINheExuccNWREE22DPyoKO%2FcKdHmsCpnUpm8wh%2B%2BaoWjrZQyc7R3mh9b3T%2FAa6onHFlwzOIE2WEvUMH6W%2BPwODoaTCVl0ZP%2BePK7TYdwWkCtO0Ih9jkNdnzAMiagnjCOTlHsZ7K3wFIit8OtrvGDu%2BClUvotxtQ%2F7pHkYEv%2B7aZj2hblC3i7kpcJaxRD4JlQFL2%2BGXnySG4qGj3vMELMnBpGc3gd7fcy2o4veDFlR1PNNHjAAPwwWeuzecK%2FcdGUlg6XNvMqb7jdzu%2FF2KH0DDcwMZ79mf7jDvYVKgLrQROEgS0smOKJ0twH0p%2FY28ftgD1GSqHRY1l4%2FRmxS%2FSb2jvHAcwDJUqm1yn0oFlPlzxfPpE6rldqlI3b%2FAebnY9FNdwCbIupwqN07GTnc3BV%2FnJlf7T3j0%2F1D9Oc0K5vxqJ6m83xz0fUJ1At16N2ZToMoWokoSD9uvVVX3BuXBYdfZ7KhTCOpKnHBjqkAZWiOXvfE5OyMboTpzo9zuJqDivAHsJQFm3ugJtGrfsm%2BViCkB7QcSMdlZhcojSPZYZ5CTm%2FqJWwwRDtKTzInnK4QgpaL5BxIvNywi2jpK4TXixdYFGWVK5AkJ6SmAQh5cpAlFnc515Nt4RvnhzNlHGT1elL8wNVUXBAVcD8%2FF5%2FJhnZVEj%2FFPyM42wGrFuWJFOtYDrwLQTwa73QlZgzTMfTLTBw&X-Amz-Signature=f52e18cb4b97f0330154db6a857b7bada54bfdc3b1df6d5dec6377a1a70d66c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

