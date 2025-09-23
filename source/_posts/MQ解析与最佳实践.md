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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NQX5CTU%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4QBcFQNj6bMqcG8paREi4kWmt%2FgjSgNa6EQjNx8cK2AIhAKHkZJ6uQfo0rqHPx5dNAfjsuhDb%2BCc2EQfHKI8uVuKvKv8DCD8QABoMNjM3NDIzMTgzODA1IgzQhFyF8D7Pz9Bnl8Eq3ANwS7heo6%2FHtNpVnYtUjZLrthCJ60TVoQ3ZjQBRCnm8O%2FDs3PJxEhx4vTNiqyDlR3Pv9T40EP5gWg%2F9uU%2BU8Q5JZ0%2BNIm2RCbNQ4Plkg0OnWuxg4cbe5gDXBSEHKgjinBmXOjB%2FqWe0%2BBSOCw4i%2Bqem5Fu%2BWcDpUevO2f4NEYGzWL%2BVYfDtAc%2FOIY2DaNhGg4Upb5XXtEqlDfmYf35i57kmHdMuNAfU%2B5PtYZKdeP%2BMuilzVh6RzSNbigsiM69jYvooxldN4Z03xfcJZiuFXiRx8d9sA7NVQGaoR8XtLG2omaHd0yiOJZ9oVfqNMBsNUlEIipRpj3025Oi27eHzva6i01rYhLIxncpdgTJivYIcBT%2F8PFKKNjmPuVdPIePxPuoAM8ztzyDbXMD5F5Mp%2BRLqDlgPJFEKjtnsk3aVO8e7RthGUUiXbGNd1pLdvTl4QyiV9DinF7%2FmPIMndmkfdtonfPLPsPXGC0p%2FxqaG7lag4lWkqobm2ssvyrndN3pSyhW15ACKo87NpUumUsC%2BBb%2FaYHoGae2GPet2oIAg6Opo88k5YiqLjCoysPXAE2wZI1rbUawhLoSPQ9tH8b4PCBNGRQS%2FkzgdrivDbcEbkqpNMeL%2F0Q6bSilDJZGvDzCu7MjGBjqkAb8hpCoCOnzsucnKv1yt9YjttmgEsgiDFeFGatSp2Qq7G0uQXa9TNZdrUZ99%2BXz8mJEjKTnFJ4k1YysEStI41JuFxBkdbsmz17x7JCroBq4nNRu04nHzP6bHOTr34ISUv5bn1J%2FJDZgE4jzsBL6I2j6%2BkEl3GKV9WaYKChdY6t7yE1fxwBqgSLWEcm7x3bruY1yIjIjzIEnf%2B5o0MMHWbszG3vxv&X-Amz-Signature=0b09c17cd06357a72e946524ecd3bccb0f5abaccf3542e7078aaff6f3ad55f47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

