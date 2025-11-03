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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665BXKYUKB%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGkeuUjD5MgFCn19HLS7830QBIB%2BQAkZd9qS5kjGvOCjAiB5tvsfjKRyRhlJpi6p9Vp4Fxj0W4mEJ9IFIjJFE6OCByr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMBJ9PsYOoTFr0umcbKtwD1z3IuyVy%2F9PiPIy4%2BgJ%2FK1%2B%2B%2BtFeKeBslmsQ54%2FMRvgrtvx1YNnONycehAu0JhlyJSbIh6V17jWBveSJYTZt1nfoWEXl%2F3DGpFrAhas2tNL7IHqHNQfOOufkdxb0rGqMa2Pl7koJdXii8RTqvY%2BwsGqVV4zbDSpYFokTYva74qNuLRZSoYZxYcvEzSDQAl1zIndnSCZ%2Fe1gYrb6MEEeVFKovjraMrsGoI932GYBINp7ePBcEvEsGlkMOG%2BGmqidJkOzui6AqA5GwCuYEdA0gv%2FXQjwwGEv3rSj1DaU2TiaBbkjUJtpdnOYwm6vQ0F0q6zZIa%2FOFETquBFXR9uqRhQeJoMeVSplSUKc7%2FAgmW84pBlh0H9BOeX1yP3%2BxKNOF28XbdR2Y4D2h%2FAksC9v9CXzCAm6OnV0ibGP4xMlKmbjmZ%2BvA3egixcO4x1ZtEblG2jdadtI2cxdY3yuWv9wQLx%2BeCue673Z9SUncYk8nkeaaN%2BHoAtEXNN93PTOCiUlOBpm%2B8nWJmvdXS006hL%2BBBbIlkPpSuYUj1d4FUZFE1TRxOk4WSytEU4zelPZYMFoRIdylXjxgl%2FLVM3CZjDTZUhIgvcOjfFaZNwvaQn3SOBvx%2FdrsM%2FzGZCF8QOo8whPKhyAY6pgHkMM6CI5nhrlPpFynDa5w%2FNG2qHElWf5DtUfgmlwZCI9K%2BgLAXZRndGYcNOYNlCS7Nv9xE2PtIrmhPSUPI8NAegddvSPhZ27msEJB5SoDnxW5LQCUC8uYr%2Fg8%2BT79q%2B59OckmC%2Fqr3Wivhe6QLhjvdOcj12qVyHqmgyJSXTzIdDbjP4ic%2BU0fndLuGZq1DCMH9KLgfgxSjKq0cQh3AYMXblIPJQhNt&X-Amz-Signature=55896bc22831345926f63cfa1375ebbecf94582a0f9572c745bbcd8e7fc0c75c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

