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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667TBHOQJ%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T140100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJGMEQCIB%2F2mNWTj38EaVSjKbdvPqIbY18qLQirr9%2B427U7DILgAiBiihVJqBqAT7G1ZieRKfwPyYnvJgksze2Ax%2Be7fp1kdyqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXFWQ%2FQjqGh9VnFiNKtwDFEvmJ3Rxaw013suhta79DOOC1IXkaLpNId4AaYa9jxY4jiZXYWQbQLRztZGSYX7vgWX4BY5eZj%2FW65xX7sYzM8SXA6XUG8OGKyCbnFArpiMKsWmZ5CoflJ6i27t1lNGmoTsoe70Sab%2BpUc52FEaSkL7IXXWvf3AZN5Da2dHfEcta60kMWzr00vRO2bFBrbhZEeK8nxywPJsGPtJmTHeXZyPqFYztHVtay47bHuNbkXTZGvANZoAXwiwlgcHZAxZkjKL5aJkrggAZTsAGpBQE%2BDtXX8WfjLAWDdHxBhk6BJWXKMHWdTZKyqk9GypLboFXO3VSslro3ptB%2FRO0qOutV2fOtxuWLqp1Xlhc6hxNd6gUSB12Tq8uFgGhkHTr%2FcVgOZF2AVNd7arKqTO4Oj3vg1Sau4dc8TS3ER6kEmzn2XBLReYHUu%2B2aCwTjvV3EIkgNC1UvTO2P8%2FXKJfV2yb%2BkBQWbjo3YVXFvWm%2Fwn%2F5nGoTtn3BFqocHs%2BCuRiv32i1%2FFWGCHNo2UEM%2BnEBerj7Ev7nxaHh%2F5IWlPhuMdf9edXoclXJD7fI8gza29fOQNfFhGnKFZKVMeCisVeD%2B%2FtOn8lEqcOaMhKz4kCuevySaHUtqoIbzBswYP63Nv8wsofOxwY6pgGuUIJnnl43lAJDrJl%2Ff5CB2I%2FeTtdczNRFcrsKwxRmxW2qpiyluSKqDNwXcckAYBFLr5BUeRuLebj9SBG%2FNhVqscjiqr1JWk7v5cDpsI81r4f424M2p%2FzYN0Zd9v0WOUgx5AS1fjbgEUlqV2fSxTkAWPzRglwvn8XkonTTBwqX3zdPIuErwRBmojpW0aB2f%2F94XlbJwIs33g8rvTiihE4tRFimbQNQ&X-Amz-Signature=56b4b112f2fb6c0dcbe4d59a07bdc74836fd87eb60ee4f75687d49ce8cab33e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

