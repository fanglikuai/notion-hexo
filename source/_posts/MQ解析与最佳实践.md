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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YXX4RSS%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIAa359vvNe3%2By4bOPFtz8Qz7lcuyAaiUeR3GDtXpDr0YAiB87H%2BXWVSA2T6AexZqGths1BuhwbBemq64qHV81eXKOSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbWTxzmOEduKgVe9SKtwDR6R1g3GJsjrWfN2X5U%2BWV4QtePVkILBP7O7e3Q8dvFOsachfDDQsAsd%2BM1hbC7yJhdh6DVujK6ar8bAn6mU7Jl%2FAHQ5es5Y15v3kWzY%2BGdx0oFZZYx8itgl6B6ooLdr0V4SpM96m8pDLPtZ3nb1STN6GS2dLisGkU2u0R0UK0%2BXkBy4IyZHve9Ge0ScnuokhrboqGRjBIs%2FV6iQ389mmDxEug5MnK8zOk%2BmAu1RCjooVIRq74rCsUKLJKwbmgutrV0GQsx3WFHnhBMVW9qsatVUumrDisDJ6xk1p%2Bpw6AR74JIIArh%2F%2BqZ%2F3ds5eyPOzLmHHfQfsdds3oF%2F0Fcd5VJGw37wFrGc4DAH%2FQFjD31bq8zrfOvy7k1zFdFX%2BHQXXwyV%2F42OJWblcUesff1ABQ5gAUI9X6Rb%2FYm6FnarUjpOzDl8QQsKhPO1F4q6CEhZwcjyj3wzM%2FxuiDMPbsrGad6DEcrRvkqY38mtQzSjOCKFLnanHZeO3t1eWdEtV6Mlrp8itrtKvr70uH59MA02R99smI5lWrbdPyJe8gvYluFGBl%2Bjx8UdAIH1APmLCMn3o24zv9m2aNM%2Bozfi34buer2c07d48q0swJ8McMW13ta0g00tvXUe82fdrOdswoZyOyAY6pgH2rvu9M0CfHJacNTZAiHF1S0lLENhZxeY7fz%2BuQORltY8cnQ9Z6n8zuTM%2BNpmzL8qHyRvynUsxNqkm%2BirTE5q%2Fkf3yP8vWi5CSNenZYNs37bfd3FFsU9gOJ8YYh8MfiJ3AHjJloF1PFY1Kh2ZZr69Vuf92XjfBvyM9GftXMSkRwTCPvEvG5xOhdg2qeR5BOxI4mWVp1sJ7y8EuAD7JFTkD7y18%2Btmq&X-Amz-Signature=22e056819d801e3af3ec137f9c11d95a90e64c5ff9171c88ceb47790b2deb346&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

