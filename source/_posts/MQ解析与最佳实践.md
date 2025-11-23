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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W32NXOPZ%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCw9hGP%2FrtJh3zT2CThxv0WlPYpMFYNKpLooaDf0k7V4QIgAggADktrOVOxvrCd9%2FAqoIUYR9%2FMz018AdHNuFw0fg4q%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDAf6owcVBdQ1PYXi5yrcA116ZyQYBmo%2BjUiVhPx1%2FcE5Dg3JtNcT5Syb3IKnj9AYpKgcII5lr%2BzeM0I6k%2BXy1BrTExuuiI%2ByRoneGbpACsEsGzXAF6Km8kR9kuHJ1d5zvU6Rw68m4GeyEunFFDzkOrja6Ame%2FFTpNNdiq0czEjjf6%2BC%2BOwA7uVN0Wpp1vFfW3m0ZaaatgEAoRDnRhNuIwTcwSpssbGzcZ%2BFRKqTJ%2BuOSBUoQy6GV1giSKjWg%2FLsrRL%2Bmba7v%2FFPTbwSZqUzHCtNO6rJH8jUd2x1ZcIpIBwambvHByfkqD0tMr3DPEYUJi88%2FggHcVGiNoFc35WBu5bw%2F7VeFOv%2BL8tkaFe2jUSY6gOlEmA5Q6cvt5kWcOjt6Qa%2B5v4IO%2FnyMhD6h1fvXKQqPFxuu2rcuqd8%2BQz4D809gUdiKUBcVvJZDi8o%2B25hPbXV%2BxDU7%2BUyOPW6ti9Jc7cjBWwGzCsMclj7olCsr3YL5xoJZDCvdWi9DcYUEfqlFXH0FmNYQqpiC8NjV%2FfoL4EV9n1x0juuNBTilzBu%2Bmb4IngO%2BaHueMO5Sif%2BGqqyXbQ8%2BnbT%2BvDsXDHfWczWMA7zYrh%2BFfBS14%2FR49bHFHxGGFQi9kgc6Flw49weSaf0o7wSKpKme3x2LpQRjMKyfickGOqUB%2B04ntu%2BOMNUfeFzK%2FyKOm8mviRlWGi4nKL4A7BsAb3eFnWgI4ODgjIMPi9WosjS0pfmvoErpk9844ceFeOphlz25QdEpQNq0nj0LQpaYT8CHkekF9VfkxB6s5FnfjntCe67Wej1lRjk7p7%2BmILYX4%2Bdq3QKd2xPys17DJmMlREm9u6uzT6tFyLJ8alUYePbTR3mCwmt72%2FTUn9iy6OS5RdhgaFjg&X-Amz-Signature=7a062ad7c8b2761dba1ec62f816654bbfe767783dbd5867dbbd2a03564c0e292&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

