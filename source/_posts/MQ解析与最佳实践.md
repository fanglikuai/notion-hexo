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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LQGKGOF%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQCkN%2FtN3quJVEde2ZSCl7F0UtRvwdpWxlxx5Ga1lGMoJQIhAP%2F56DJ3iB8j74vVHRuLsqJWa5za9WkCRy9J7YIk3fN1Kv8DCDMQABoMNjM3NDIzMTgzODA1IgyTV6lspbPNOgLzhtoq3ANNbdYMyCWP56g2K8nGwxT4AjrNbOEqgMgBZRRfbYdi%2FOIMdqmlVbcGWnzPMDu3l8TZ9wMuX4GYD7DhLovZ%2F%2F2Xe9mdb6GFd7S5EcdxFv7KmuxYOvtxo9cfE%2FDQhdVrqY2jhrU1i%2BduVLYjFxM6h6gwIdUdQJLKEc%2Bgy894G0GsN1ZIZX0VwctSkJeZbtKbuANbkkgxRcEsLZOsMeJjvRS0XE3BIaxq%2FNQiA%2F7i%2BnsP0hhwB20O8C8%2FPY7E4k7BUdXBnonVwK2MI3Q25g0nnZerv2cBGcS1ZFJIfNetCx9HbjzbIq09U8%2FBKbUJQ016YBAXPRPzuq%2BlgQBmDXHZvev3qunOSMhKCTBsEoyYOczm4dC9hJwVgLh%2Fn7t9hp45uQqJY4xyZIQIXSfnMzUquNFI9rSg2%2BE8biHyvP%2BqPraBOgiTqetJFAcbGE4shyynEVSfX5SxLHmCOaQdEWAohvNdODSWYEIa4d4PPyJ3BkEQaKwwYOwHN0eRjYyLe3aNkY5ey5nnI3aCRFQaiJSn9Gl21MiWot6R8IDc8dhJ7z1s6kMmQiFJvcnSqUmAQGwRihEbh308hbEit0Q4zrJdZXT9Ix%2F6fr06bFGX7IcLVUrjAT33Atael0oGhR2QijDHt%2BTHBjqkAb3zqAOlVq1fItHGRSGM1f6xDnX5JrAO0biapnDZqqVTB%2BI1AJFAoo0LyEIJ1bj68BhDbd%2BW%2FVYxZdBxO2AVJmZvHyK2ksnYn8Q1l7bOs4kzjU1oWwbqkW5E7rI8Nne%2FJbZbNDiYuXApknWHCi6aWeE7ybtUBoUAPv2UuKc%2FOpQcUhcB8eAOnMRmZVXEvNnZ6Kboq5v4hXe2skAzg73XuB4wYqmG&X-Amz-Signature=2e790ed47b8ad1366247bd91252a3276f461c67b7eba7263127ddc278872cce9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

