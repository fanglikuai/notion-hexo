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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664YAN3FS%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T040127Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJGMEQCIBEGnFOY2ha4GKgah29dAROmsDsiwt5p4Eht5WhQK24AAiA3vO%2F3FgoxcoVBhirOscBSshoUf8l5KyBMyVWJjrBAsyqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM410HP9vWAwgxYT2fKtwDTkt45dPOWh%2FzTCtq3y3OoMkwOjcl%2FX%2B3FSmJq6MYkdUydWkPUsTDmo1EVrWN89VPE0o7VntooF5yGLLBMviyxkh06m23iDmWaNIxX4pc46xcYTOez6U9QxxbwFoqu8gEFZcD2AU5JNEs9HUNz0JrS5Wu8JsqTROFlLAkABxIiesc%2B8bow4wq390Wuf1X6FLwMtXPxWdyI6jrRfWyWbsMFvV0Py%2FcOVYGkSf1lrCXjXEUT1dwPJzEbat9TQ1jIf0zvzEKIAmR2S3sl9oi3wK1wXRPPc6T42F7LeKVwITcmLyx6x9HX9Z8U4MxcODJ3PKw1PUtm1HXEm0vg7K0buw%2FBlbqfGN3O5aeBdRxjcWZ7oX%2BJ%2FebXKMYnMH8lXpV9nkYoDp6hSjjVHhqLGngldwg1KY25q2%2B6Nblem358OOTDqETQfnGpUMFQIHiLN2nvcYVm17y%2BmWnKAMaDmUByN6bvn8X0ibky9HaP%2Fi0fNtRH8jXcsGg9kLM7%2BHS%2BbXuSTXkyfjI7gVwfmIA6HlmcVBm8M%2FsOWEen6PlgaL6NxQgQr07wpyJ%2F%2BCBn5IZQMoFsDHaNRbcqLw08jK2UN1vvTuorMh%2FAnFWcOhDqfGsRUCnRNoMv4kA9so8lW73GkMwuc7yxgY6pgHhZHUTuTgBfkIBTdEsyrBk7VZ7a5CYCi0KxankWgSn38WCawlsy7yJXpiHnCN1cr9r7xbOJccFV7h1q6oNFRMlPShAv4rz3Z3Gk%2FRIwIawpLN%2FbgiulDdqvqiRidQ%2FXNnPXaFpYEr0aF2oxAiQQCjOp1Bg6tjQovIcUB0Ka1azOQsCW7oRZXehvj2zCpE1HdpAKHGRGhhS%2FWUaqMIQTjMgE0dA8s1B&X-Amz-Signature=eb2349b1cb9f05cff3450474c59f5c78a8146692d7351bfe8aec0fb73ba17c38&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

