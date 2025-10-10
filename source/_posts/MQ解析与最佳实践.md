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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDPTS52O%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJIMEYCIQCQH8UI%2BwiI%2FLrjk4SPxZH9CFoGhpiRMepE9hZHjzp6jgIhAJgQjxzrscF%2FH011Aw7imrElpZfAsh2gzbw4KLCtzwOcKogECPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwKOT9I9WSbTks%2F2CMq3AMZuAH0ypYQohYh1iLskM%2FfUdtxq2qurSa1gH22B3%2BvccuWGHdxMw6jqEBYlA5h2xAUZMSvN%2F91TTwGrpkJ2oeWRrGpTsBT2qJwHmvC5Tq7%2B2tPrOq8J6KNQ3sSQhqfcpv0ILTHu2GlEt0yII8yPx7nsfDM6XjJdfsvwOWg%2BQchcCwsTIOykx2XbJ1vlOfsBiEkivqR21ikn0GOOfKliKjcNgtEkCJn9AEBPCzfBAFDMkXN0uqiVfYUtduQVRKcE5JSItIIRMhAWk6V8r%2BjK9Pe%2B8HQaIrzCl0YRL1lHFG7gKb%2FF9%2BXvtkfM%2Bwz%2BmTqnbf%2F%2BoVshQvq6yTLqIJhSDLZLYvAPl4sDKGAcRX0jN2NXpDCZuPm7rQjGVxd%2F18mYYmwJBS13njqxR2jHaGkog8eXzzf%2FjSQ6qe9smq3SzSOlj3xNKxqF2s4LWGBaUXCYGKgbFyIFoMZI5mHDVQKufVj%2FFYfPJuzVRyVYYh%2BkJoqo44zd59T8UdSawAoGk8LuQXP%2BOKof66Z4Fl3JYvGBm7mT3qCZR0dPBQ4XyFZoG2Y0v79Wn7fubtO0BkQX1LiO5%2Bs%2BVOR6KBVXbg2bjDJhGbYGzPDBzTx9ahjHS%2BIDFGhkiWncw0GwPUjkWD26zCIvqXHBjqkAbtZW6oc3F08ArouNhrPda8w7um8kZ4hWWfnmpAl5MrPRuPzAi06zqi697k9zgZctIizWUtWekrWNaf3Tnyio3ZuPVibtS1RJmoJHaAAdmyKGbmKsRW8ATWfCWsnliJ4SEDhL%2BmLQpVZkQxPVw3kqlNZlUfJjzCIEiKl5zn%2FiZM693Xpxkj2CbR%2BAJ4yNidx1WP6h7sOt1e8zxk0sUDMjufkaMhC&X-Amz-Signature=41a52ee93e3695b4892da1d67994be2409c5e4016da376dfb4090ec0e031bf2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

