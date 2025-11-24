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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q62CNWDT%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHHH7yxhjL9kOZOS%2BOjzDUN2KSOSjggzUEaCi6OC%2BuKdAiAOH68d1l7UwBjB1Kdd2wxvN4zS8rCg4M%2BBuvSVYMGODyr%2FAwhXEAAaDDYzNzQyMzE4MzgwNSIMg0CWq%2FVC%2BzHQ8vrIKtwDKrK%2Fe7eaPxQQWYVslNebmK3klJkAyASv%2BPDCNxuDRcfQGcbW4jYzL%2BxiD%2FnkK0Abouu%2FHfkDhV%2FJV%2Ffc7d%2FfBqrKhcctSa5AOgABVx1qnLh3hyuRAa1Ls63FczGpNYNMZMQuhzJc6tP%2BjOrzvny3usZSPtFU8OUR%2BbJTnz%2BqUO7gq0B72%2B5gTxmdIpWMHl%2B9bbNZLBBXm2Hhg6UstJNhPN7mz0vZc7xR8MABvmNLt7ArmZFH5nXndwEetzNjDInYGncZpK4X1Fw%2F%2BQ4g5WJk93KFjJBW0bYJEa4%2BLEMks0JFBnkfJwdnlqg27i%2Fps0uoSqb%2Bhll6AQ0v667VBIPqCOSRuaCxS%2FM0%2B81kdk2q937q8Mva7APomq1AwgCNi7S6%2B%2F%2FoqUTl9H6EHAS8xNLmFrDJ6%2B13OQukNSLQKzOYTtOGOM7YQJvsQGoZGnkyuWf9kRsjUaKdELlrbaq67BQnl5HdeMNkU0p5UIcGO7mfkwrPmjkVcU8eO%2B5q2dtgy%2FqITiDZQ0nK0hm6J1cZvhXc31kjjQf7Q7duajZYWBAy%2FrH0rCEIZga3nhTS419n6QNJsyL%2FJviUmYEBV0a356K4zZnpbmKhYvIw%2BAsM9lUg%2F%2BAhyjeQCV%2BpTEOXUrIwvr6RyQY6pgHI0S%2Bt%2BTNYwrLO2BakGF2pFbLnfLvi91pZUQWeNUqYAmF1O3Ts%2BbWlQGwdF%2BJi9JEhD97KLHz6haMQBihA4lDSzL%2FVF70AtsmEMsChs9PwSeeRrWEXLddQ763rmUDkCxMBNfFRIktcm9q0An2XfrQJ4FXhFO40hJ2CftZ55%2FGdjZmGkmcVJSD4J%2B6P4qnlu7RatzPzQA9jguyflGYYePVYHQKD81sm&X-Amz-Signature=9cd93425cb0992b5fc8fa32ae9bcafa0ccb36680327430399f2a9e3a64cf22a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

