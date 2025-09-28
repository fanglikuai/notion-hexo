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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRSLPEE5%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJGMEQCIA4GKiGcrBW%2B2mXLrVq3SLY2cEjRF%2FC1%2F9xvznFunyyuAiBRu4s9vB0ETCdt8w97dzNb1MEGeVITVSsBNDA9i2N5CyqIBAi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXiR66kXR9Ld1pxz3KtwDGMAHKkinEYBpiZnadwoEFRurHSpENBIyxK9EUFr3BKISHEd3THaiYZSjGYLsesA7wW6%2FwoEI3qL8ceL7nsCeFVbrDKXymuOx%2B9mWOgUBBiWLJTjx3o3teN2cnSLW8aPepbXFc4F2EbfLDmJxoWz62Jq%2Fu0C9xr7hxLOswkLGmKOsQO8XWK2YjwEhQ6NBMni4w4TJw5pNcsuUR4nM%2BDln2ZVFH7w5uqD7mVA9ic6LL5At7TNmXKjZQ60JFdnGrKs%2BaOYvzdGnxCltHXrWkX5ESgZLDbFGQEWIDy0a9DWrvlLCyQwjzGhE%2BHmqSPW5IjqQXnbKd5nYCOkdJ19wMzMmI%2BaIcUO%2FBkdNS3Z%2B7auIsboUeqTVB0NTz4AykV69mZrINlWa56xffPMARN3BQYxMWOmaS5p73dEeqYY4yTOrhjExNsjdB74fl6zrTvcEsqzzdEBnrkIUc8FiN3FRNgC4Wlmscqs%2BQn7I3%2B4arX%2FWBDHD2oGMLUgZGxyWv%2FhoTGcH3gaJ0ugqC3cK0547yAtB1bKnsNL%2FxZk5EdMQkupcoFekdM5KxF5%2FLuwBWLSI6rXnegQHSM79OS%2B0su%2BYaa7DPN7kKKEuCHTtGVQcktk%2F1mmZqpQcIL4ZSy2irKIwv%2B%2FkxgY6pgH80Ius6RhiX1MbIsJq801dZO4ROitn2edOZ%2F57u55jmYxYtwXPMmDiQF4Snas%2BiL3fvwtsGfFyMzy1symZP4%2BmBh84lHwwTtQNiERcO6e%2BmYZXouHOcCYaRAEEP0RSZzOXZZHI2ISReSupMJwSETlGjhVP%2FLFAEGFxsJGIWq10U8TW5yXZUtwbBfYZS%2FIP32xmlooMRq10mjQIsDg3oHfZ5nWZXgPy&X-Amz-Signature=279d34f1dffda7513d27b3bab2cc26143f53dc6093461c0113ddd6dcf8860117&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

