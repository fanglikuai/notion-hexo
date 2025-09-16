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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMFBIF2P%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T062457Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJGMEQCICRfd1ApFIYf20TLqkPQcgbaJ9kaUgKpcTTY4ldsfO7sAiATj%2Bgcg%2Ftyo04qyzWwnRTkFGUoTqrK4M1w19IVx3EAFiqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BtvShJiTePrXnSJiKtwDbV9KQox%2FRYDWI%2FNwe1r9kO71vR5K8RsBySpUFSHFiL8xmEZupLST861RErpvqfQSBXwQgTKpX%2BWE4AtJzRy6p2kpbpy76wI2R0YIBBb7%2BUBiGfeFWu6PHd7qy0ANrhZYco4s3jxdp8J7LhnlMKA2QhCOmY1lOeTtg1r7CkWuAgJ1BDkxuVppHVGHRyE%2FlSL2kv5%2FSWQ9JXVoaltMKrXCPFsBbKwekvjjH3Kays%2BaRbjGluQ3VtXi0dwJI8nRrtL4rUqR1cjsIliZ2cnb3BpYS8ieZR4426qoPXBNSYCylKfg2V9NMvi6T2tAVnP5OotyOrLliH86synAduwRWhYMwT8a2%2F3cPnsqNfOv6Ih6RbpzM4TudE5zZUZXnhyK%2Fg9ywmwhkbDGNtxWPfsaaUvUm9q%2B4na4k7I0Z669vf1fih2Agwx5dyCQR3COm3Q4P%2Flf9vZ%2FHZQesmXEnLpfoMDbHdr7rr8ndqfudLwy2Rr%2FBwNVHdY6jvS0DYznj6nXjmPreFRojYG%2BeVg2HJ%2FNVt0ydjL4Ddipep0YlCDm%2Bfr8BjNAXwSdf4Ax7Q%2Bir2I%2BcCQP86mu6VFE2z%2BGts7HC8qdY2FkSLb22HAxfEe8okcYZV482BG2ncrJQ95Yx6gwifWjxgY6pgErOQ5CR6cdTTPGpcSJZcKVIQZcN%2FzHSABFp5rOUoWzZu7GtDNLXziFfqZVeuQQnB7pxWcOpg8iiO14khqQwQC1BpJ7cVxNID9OrnlRjMGgRU5SgT78OfZXCaBA3DyHZamq45oIjKabxW6DubeVCcYSj2xUh7euu3dlcWcxKQMpK9H2qBznyhfWjMTwLYir3wlh9H3mXy4u3CfsS%2F5JaT%2BJ32u%2FR5ck&X-Amz-Signature=245fe254b3e68895d4268572dba02bf037bcb4b1e3782119a7c45908ca364268&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

