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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJ7OYEPR%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T160050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAhNYZ9Q74PNOiIbYgXO5CJTzmNvr1LIfNCPThr1h3NJAiEAgX%2F7gNIQUKBFCYowq7fYZSHjbWXQ8qz8fHMrNNFHvekq%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDM369VTMy%2F6zA7AlICrcA4l8pChqZpJA5FaV2P9ecx4JbnWLyymaXLqe8LKWd4%2FJAXqhUdMGUPOvIijjRg%2FzCdlAkPWoz5cMpMi9KDsXnO7Ch6BGOE2Y1hXmIFaKUDrXNupueYC0wTdXnfdJWqx3ucPqIwUmZ228zHNqktPpaKK5fjxxzGnpormDaaS4UJGd%2Bllf6G0nR5xiv7ve6XA2s1qc%2B5qPZKVdduYqaE9vLOlPxymlq%2F5b%2FhuOtyupnb6CZAYkvUtU3oc0pvLSOACQ1R6MrEhvQaJa8mlzZdMBcyvYwrjyS1VsLuYCymJiky59sZiMErLkbtB%2BGLn6nkDzp9OoSuOfMqTjftYjOD9XjAjWT5zBZ0FM%2BGrMgbE6d719XDjKwzELH5xiGURqmYzl5%2BDcTWNLLD8vD%2FSSYkHhmi9nmyBxTn9zN%2BMBB6AZ3Thupb%2FU8IfwMz5RUjyjRSi6LC6hppT4vYhc%2B%2FXkP4Z%2FGJ2m8AvgGOpTmLeN8Q07CQ%2B68yoA8E7y8C8b6h%2FM1bqftj7C1Wla5%2BoNRidngFQ8yCDfCO%2F3BXaZqNlo6B5EJJggIHPCd48BEATds1HsGH4GC9%2FO4yDRlU7BwlwTC%2Bw4wjcAzXpUyLo5NCRd%2Fw1gSG5Hvz17EmPF%2BUiD%2BrbeMJu81cYGOqUBbNJ2ivZiP0605ZhM%2Byb%2FnIDYDmytH%2BNyEiVBt%2B8cW5gs6LXEog9PbiamvysGbMQFv4N%2FPplb0Ds2vTjQLacMTv%2Fs847o%2Fkr0xrZL7GrKUv2S4Ig7P3sVjHa0NIxESc86Vu7rNjSHVaLOFhA2GEMEC6DPkN2%2F19wBoMAXhD1hqZryGR2lxmRRLXyYKzXSyhbb70pfQKEvkO71HBL4%2BfQt0j%2BsYUO0&X-Amz-Signature=d7606a539fc6f5053bc5009c9734d5be856b5fba272e225c00ef2a88d7ad22fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

