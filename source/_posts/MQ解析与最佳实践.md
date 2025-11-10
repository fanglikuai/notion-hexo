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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRNRQ43U%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQCr7ts4Koj3adkfutEQRiA4kT2nPoiu%2BXIqm4d44idW2wIgap4LMFoLC2nNkAqdZbyZl1tkiiv17Jr1SjOH835hhT0q%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDGJWwnXziZw6oT3CVCrcA%2FZJk0MIMQa%2B85ocaxi3nSc2JtWlcOLt3NdjpMUGYg8qDtpq5AENxgYAj1JTidT0VOeomwI2fbVleeE%2BjJZR2g6TI6tVFmiOAcj%2FXV4IS6xkVZy6dScUuWQpx9PHSccQhJNZUes0AaHRXdMQ659xmQ%2BwEnFXOwCwNkxXXeuLCVY0%2BfBkpr9bTiLLMPEh%2F2GEJucWOkRarZyRF5Cz4reXiqqlbweQWS3DmKIFdfffiSaQamTvh%2FQpKwZB8WFNz09QVRSnpiOCwWKGwG99%2BCfHZNaLOm8hK%2FkPeo0LlepQTcBOyChjh7TcN4KAN%2BBSJzMHzp0YIdoGdRunfPN3yqtzEf%2B%2FmYdMczyWEm63fAjpYbFbhEavl3ccDAZIlBSbTebWu4oCJ27mJ%2BPZBqkHV1aZf83J6bxrMLcLNt21A96EKcqIKrl3UK9Uwz1IAtIkErzyDi2OQBQU59AE9mtAW5nAAYoHGxjYKsUPVJq9XTHcFjKdz0HDIvAkPmOYG%2FZjBuGF%2FcDaPAlvQHtPkiw9kM3f7qPXBr6x54RZYJXtc2AfTvmOQRwdTOAj3XP7Z%2B5M6WBfzJ2ekc53ikSQSj0kEUX8btX1ACCHf4dsXXxmFWcOiMElcQPO5NhBN%2Fz7Q4WjML3LyMgGOqUBGHk%2FJzCU5pQ7zeUaCZhc3ULykK%2BFvWXWfHInD7khanoN6pP%2FaNjvOaCaNZlVqfkZo6FO%2BQ6jTcz9redHobuLYq4nM4ygHtansY7f%2Fk44MjnDhEXXOSoZbWe2ZLO6Kni64S9wv0aBuJ293V0ML%2Ff%2Fwjjvn6FEmSdJErPBRIicGr1Xpek3lytSTNf1GCTxw6b9npweDwcZF7fKW6HWoTABJI5caFS6&X-Amz-Signature=54299091d296d925003e3991938c52bfb4fcd6793b4bea02971b08bfe4a32bc2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

