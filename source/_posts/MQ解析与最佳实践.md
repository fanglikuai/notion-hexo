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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXXZUM5K%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T170040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCBlgyPOKj1WLJ6SPqJZW3RmSGIwM8TmI%2Bw44F9f2k67gIhAP0cgKet8YZsaiQVkef4XrClbxLnuiqTZaW779c%2BcdmoKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyaTDMkILzinDrA6WQq3ANwfVs%2FAeiUGOKah3BODqdM409OqAUoK%2FJLHjmPCavTA06GOgFJ3z5YbexclAWctADRJ9UW9vXUG2hvnVts%2Bscoczue%2BVf9kKKIeAoK4giwXe4XhQ%2BFVvJk91Z1iKxLsmxvB0htOZLFav%2BQi1Ah0ivAtDhN04gE26Isd7JhCPUOMiMy6F3GcIL3xnJJWBsCOoRonZ0vDIk32eOKS9bEOcIkf6p3TGGTugAhp2SbhlhUy6iMyuEApmNG%2BQ%2BVBtCVF%2BF3A8wR8xKJUWz5XnMucJwufxflb%2FHoTcsNclgcsQYNYw0Ea7CJoU2YX7iG7GU%2BneBkJ01s6fZvTBRotgc8KJ144Bl2klGniLlDfKKqjyGv9uQ%2BeRWum7IGRLDirg8LdIWQk4LEfMr%2BuVSKm63RmgcKQXHGdHAoGGMrwV%2BTMV09hRQal4fd6Rzagl1%2FowlaAsQe41uSSQIUAQB2KT%2F%2FuQfyXrqoVgjrGK%2F4TcMH1F4LfIejfgZyFJNh9C7QbDoRtc7amljBjMfC912bn2%2FccObbNRg71KWr0Lr4LiOZD4Bu65STU9HwMbiQ4SKKXWVzcLAYuzGIiL63hNfE8AsB47kHeH1Q0HnuQo9wCPjybDpTh%2Bx8IjtNyMQau%2FdQcTDx%2B63IBjqkAQWvasogboXjLP3hcyI00m3PwpALuKLs6K4xUrPbG0OHUBb7W1ICOygccS6T5hLmR7vvI3HHPRs27LLStMNdD7TEXvo9DMLyDIc2OOE%2B2ZUttnFuLG68hTDpqA4C7LZjPPUylj7OdpaCI2Lt4O38a2nAxgUZ%2B2QvgUNltZO7OFKeAEVJOdgVF4FrGySc22XFK%2BwWuIbpnaJRdmxY%2BGGxkr0YcOIS&X-Amz-Signature=65ef8336632d0c8df54f629628abbc63ca1ccfeec4f5aeea65933670350aaaa4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

