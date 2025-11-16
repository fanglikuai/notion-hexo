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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYNDAFRA%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDqHplyP1TrMCVgrSpaAAjjxytjTia9HImvmutmEGUUYwIhALQtGPNF1%2FLNOyN5IlEfH2NeMGDxh067mzARiH0QLS%2ByKogECJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwhxyp6KPVUSBvp3Xsq3ANGZMx8kWBNYvscdK6oV8HWnv%2FufoTm1YeNDfzbjPUyJAwicBUiWlWt0g1wu4EBdCENQE9ZNKcRJaYT%2FaWLS3dDX7L9vVq5s5ggPq9cn9ygzSf8pmh%2FOVAcd03uNAX1TuPn3ekg4rjSQbM%2Bv8OWYXKmL1GpotickKaFsZXftXekQikfAIw8hM%2BEtgvuq4uiNRzI%2BrxZF4buJ2x7TpfJiTMd6sMW%2FpH0kSxlWr6gx6FE4MhB7T%2FRehBhu2iXw53EQbZyi5wlw%2FJWtm6gcPzUgYGyL2szbeysFKqSGNtYNlp7p4zAFsh8og0KA1DQChvh07tsC1Peds2QW2X7EhRtC%2BAOXzL2IHqKe8DlIePbV2mT8sXv%2BTIQ6HN3nbYfK3wUaGrhACrWAlYG4QSlpIPDzaWB96%2B7fSqsh1Ofo%2FwMmpavOWpEEhfHQUfHNW%2FKkQnhJxEsHk9CVpH0kLmVuCEGGW1k02mq6qBnS2cMH%2BsNAgyM3g%2FSZx9Nm9fCF%2B%2F%2BKbbWirnRC632Ikv6TYA84BuGuREN1I0vlaSGIDyrK3OZ3UIFd9ZIqEaAaBQ9nXnDUEuzUwR5hpYTXhnNM5Kb%2Br%2FewZD%2BraQd6neivg9utZ0KtLIk3Vd%2F3SHyTOcr%2F6RSGTC13%2BfIBjqkAVk7YoND%2BKHjjN5RNsNd%2FNSeWfKtOikTeB0%2FKtZ%2F00sV%2FsMgoNLHcTM3PQSJEDk4O9ElwL85BbW38ICFIExH8H58W9f6%2FW%2FlauNGlXibP910cfmfQv%2FncSA4sGtVhCZAdgJPv20pHftdl9LO6eUjIlmmKWjKcKl4H3I4F27x5sPO1dEoufzPAc8ioGNCXEkvsmGpqUW8s7EoYIqDSO6oj3NYhxov&X-Amz-Signature=16fd0a29cd45a1eee1ff3d183ab4e88d83642d58415eabc1c0dfe3345d3f3d06&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

