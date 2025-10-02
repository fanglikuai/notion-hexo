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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ISGLUJP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T140113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID3X7ZpbSwj%2BjGZw8l3U0iTKGr969CkSDHzM8DXxnH3KAiEA5LbQW0tD%2BCIK80m7ko%2F%2FMtrwHmPWox%2BGgtS74NQrCIYq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDJJbu2xFSnnsIP9RDyrcAzrh4%2FynrQ%2B%2BciKneLH8nY0UGpO4zt2XgnsC2RodPDf6SHb%2BGjsffk5bXVQZoXsW85KjAMI4CD8ofM5YyvM6dfYvgX4obH1QdG2uJ0ElsXWQaZIGUH%2B%2BII34Nv8egQ%2FWHUKjP6kDBYOTwavTxKpEv7jWoviySIJDFQJjT2QoDmW3dun85pewlpF5Sd4iQtAQWaJQ4QeiDmDvshGFYDQspSyCDRnciwx2VYdGnLPC5AaQts6ZX2bmdKV6gmKPIKt7vI0%2B0BEnAAnFy6OJzGhpjvswMct7XFvmHkxYysbOeXhxzVHzRNJBQFGmztjsj6JK9%2FvfdfYynSIFix0WIa1U7zWrqNsO0mi8YlpQYjowfmEmLzFIAN%2FUpWA4YXeKcm3GzE7zfzS2DBt5W9uOuG0g5FxaDWffP4LgCJluQiWHp0HfmRfkwAo8WojB5r9ZrfDPvNeJ3gEZtpB%2FSQ1iDQDQE74BEjZe%2FJaZTIRkPxrTq25udejKZYsiAgv4dEkslZbEqO0Xdv3a1lNesyqTjZ8veKxanKQXqgqnAxvbvkp0xatMVdXX8ag3S%2FyYTVpWHiDfH1mvTkQZ0HIhYO8y5cg4hJ6%2F0ewwkqN%2FOava1GUXbhMCAqxtxoG6D7w0bWtYMMvD%2BcYGOqUBUbaGqVcms8uuIwBPRYU8ylvegGPy0%2BgwunQh3LAROWx0s63egx2Baxdci6ZiMaryfNVGVhIgpFj5Mfn0vt3wTjLqalbSs0AK9X4mVlgggGNCF7E2tYoN71c%2F1sfCKK8JwEIsHt8HYRiwDktpwZMNXxe5reNmRF0B45Ej9FQGfPhBiqcA5593E9h%2F9A1B1GCDaAlst5w3zEIR53GFoBEkDNoDwvug&X-Amz-Signature=ae2f28add22d54f0bbbf387f23a3fbf67c6d01c010f2a1c7d6de8a1708ad00aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

