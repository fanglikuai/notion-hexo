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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663V6GVIXI%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDEA1TRigjg%2BhaBMK0KqW8uuCzYTf9erfLSSXVLF1jRGQIhANF98dC%2BrZ7PAF42OlUV437b8FfvLGXs%2Bx23tKivWW8wKogECKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyhGjDCrdIWk7LCQwYq3AOYNPy78L8ZWbtKr2l5z1Q%2Fc83arNjujeBMoZ6iOnr63CE2C67Hp0wm4bqakwKENIOG42b9NLgfHnrKgonKCW74NbZMOfUMIat5VHUxAs1uMM%2BM5vJ9Qi1xVDi1AidIHy%2BDnJhfdtOP9TPMvZbOkkP9tBmqlJnZk1zktqdYuDLIS%2FPa7Wo2WE50w0YVn9bJ6oMgXNzijFXX3twttB7IVRA4rY1FeNPYvJFeETCeyxflUteXgoUmNUaGm%2Fs%2BdeETbYDba3IHsLOo8qpvx1hb5g4NDebhG17ZpOH3wJ%2FU9HqYRr702IB%2FMo1P8%2FP9XAD7x8DuNYm9nrVxKdQ%2Bhr5D%2BGpOtnlsgb9jDO0Je47wyJbUSHKOCKvWcpjMtEdjzu54Fnj7NQkfnuOvKv0NHJ8Nyh5B0sxOs%2Fbkq88DlSfxDRY93r8EFPq3dzxacTF2MkjnYIJKEIlXUwOQt9Vubfhf47CD%2FB%2FToBZ2xuBIyGqwA76y3KUKYg8jCV715EGkcZSCuPvXtgiJBFycgUaRuJGHCgQhehQT6gY%2FPqa69OwjCnJ9O7joGCyuDfnDhM5Bp8etb6Tc9l7Zq0gSvPTNBonhCusumK4YqCM7sAbbOVzolKQ%2Bhjz3p4pN4RvGI6XruTD33cfHBjqkAR5Ot74r0138C3YYn4qrXsnRreqgFNTWqM6xpNfauRpD1GZUjC1s02oR08iUOmONkT%2F325nk2dX32riTJjjKPZJ3ggo1Xp5nx%2F5w%2FBhrGsQIvUpENRKv8JxDqKGRMGLPnfuu%2FDjaNQktdnv9krIgYaEAxgw%2BsE3otD7WszQZc2hJaGfCsmuz9pBXEtJQkam5%2B88HlZ%2BhOzHvCYNNLRpDrGS2td7q&X-Amz-Signature=142c5d48cc15579086df3c7d074f0c1bb677efc53fe12afaa4014bd223fb005f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

