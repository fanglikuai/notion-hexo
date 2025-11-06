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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQN5Z76S%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCRJPxZPWmoQb1tNI6sOG2NtukSvGIryYKgcehcv0r4vgIhAMgiPHcR1VzS9v%2FT6%2F3DBXqLIWEWuOHOiSAaGEsd3iSOKogECJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw1nsIWnTFIyeMbdU8q3AMNpiMP7pKpZ2wm5e9iS3KTj45LPLXmUkz66LO3hTx2AkGXoOTR%2Fn1byz14shlpswsl0M3OPsqNSLP3KQ3PwlvNU8cVLHmLcXyEG5W3JFhUmT4lBhLZS9l9IYaFqoZUlMuvJTBgJ9KbbUREZ5kCYZK0gNB3kxjv0gksk76q%2BaimSlMI4kK2VAw9CqSZgRjLKSbqSYZrHxfBHSNHMB1M0wSChrAkvApzpyFWRL%2FQkEjFE8dwdbv9bpEtR8S4BETAfLb7vlIErLIJXMP4klto7S71ci%2BSnBNpVI9O3b3%2FtNmfauaNBtHy3rME49RSvREROYb%2FVfRzzM3sj7wE55pjbA0pOSXolYL7tTkHNdpZeZezuc80tFlsh39nSvlNlEym3W4YUlDlUaM6EnQ5RYt9hmZV5dLMt16RztoIo9xeVOyCvlT7Y29lMgDP%2BxXkbwQyXXsfgRgn48IzxCkNUCsQuzRn9OLPeJyAT9xYrTr7QUglrzpueCeGwYWjSi1k1IuyUGLAqbrFPN0kJM0eCMVB6r58RJFHaVZ%2B4uXvdObBnFWRJ1GNfcCbi5gUkL5cZHG4wpxb2f16NQZlyJXxIj8NbczwBbkJUC%2BPUISY%2BOvZcLQ6EhLwoHUZGBpAPNng4DDarq%2FIBjqkAUvoPA0gJskuJulESedXdOXYoUpWVnnYTjnOuxy4yCxIbGxmXKzLBHhyOOte4TvExieya9rYpS4%2BQdGHFA28%2B4IwffaXGiL7xkxLmS6kGGk4SYuXi8CkoUcNfe5u4UlwmBzFnrcm4DiCsySlvuRiQgUMubS1HQEmld5phD5YDvw%2BkTZw05yuLChPxjwHptCNkCzEmGIB8eGYd4jqs2%2BJ0HEMlLrG&X-Amz-Signature=e66663f76237e4f361dbd99c38a918ad4d4406dd0276866767d361e3d4c4d639&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

