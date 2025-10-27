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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466346PR3IF%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDW7tiKmLbnhUs93snAz8%2FQpP3E3sao8fEnuo09vzWB1AIhAINDWZtbuC371hHifXKtKYbKbqhOSbzqhyFzHOz69n6%2BKogECK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx1h1hpU4rx0mIQ36Yq3AO%2BvftAJhpqnAhr9TiV%2FTEBP7MuaY64ANn8PWnavZ4nxEs7pQ3KALZAmJTIN9UoSw7m1V1QUc%2FLZ3e1qnwo6GEkmkKSGAmoX2A3SA%2B4hrSytrW4dovUnsSjmpN1%2FxfIHItRQdBO8NtsM0Nl%2Brb5gBuedyrsvkCMcp4GzSX5jMGD8O1aX%2FhfzJJiVVTlnNc%2F0GFjI8nBAf1GXbRTXygHA1c34SYjYlmOWBq00GeUbPpdXHnN4kQ0jH3gEFFd1Fdu6wfAj9yLfvhv%2FJFhu%2FrTv4KEe9tC6QzXyjBHRQ2XlofZXAeimhU%2FgQ4W4DVYELgoj8NPa82inck1VzBfJKQ%2BD9EPLxvnLcbTt%2FkHEg0INlGCOvYIPCKgTSFBmEblrBFUbms%2BsvlH97tdTcDjJNPQ5Qmu4WrAMYe94JEpFZh05AY9FalPBKHaCNpO1RrSIoICNxtVBKJ%2FNMScTZXurynFXi7BcVoJPdMv%2BVbtOlrfYgpL2njk5KFHZm25AnLJogGCZ3OSgEWt0N%2BYPiw8m3MwaFXzGcQ49b%2FJY4o0F0YwBqnXRk0S%2BAL0KLJ3FpbE0ADzmIXWD1ie0xAPBrz1x4riMqTt9ti%2BD08LAlPtcwYCoFC8ybLuEug45zdEri87EDC7mP%2FHBjqkAc4UgIQLVKWpdVcaUK0M23SqM3TkLKNCpiXODI1v1Ljqvq97zOKF%2FUki8QYu8d7qd0gJEsi1%2F7poOvGpCYT6LJJo8UIdxq6XEWRMHXSzZT0Nh2qjKMwIbOD8NoIN3ZZpFjl7gdjVNp21opaYBaNp1n0Q%2FE3bb7erMgnY1ECXPHK6Za4LdV7F5NdWLlvduX7ADf0p8Jq%2BqRHXrlOrHclX5IYMGzKe&X-Amz-Signature=8e5810708e123190dbc00b56f34bc600df792787797ffab7cac4e1290a942182&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

