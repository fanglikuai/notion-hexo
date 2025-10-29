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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPQBENZJ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIB0Gzh%2BGTibuCtoMdoitemnITY3CDIM7bVPufStkHaSNAiB4dLD4iAbkY4EjHmxR46r%2FPijl8uLey2ctni2Xziaf2SqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMz%2FrmW2VQFDYPau8FKtwDscgLVM1OVM%2BDsb2WvVMugqZekvaQcpr0Hu14IkTxt2LJZxq3P3cMZUQuTx3x50ZFQY2zKTcGY4ChfBhDPsKvmAO5oYQ1n17h2pb1uST9qMq97%2BrMu3dHztrV4BD3qQADP8oy4idNazeiB4uNROVXxpgnpYeaOzHML11kwbqdyv7JmzZbOErwpBAumeFC8INZ5Lybs6X%2ByqP%2F9MfqQFPCj9E8ecA0gbwpLgDIXKnz7IZNYFghjSzzSfnZt%2BZ0rDtcEUVTQG5gTJsMGVMKTIdOQ9HF%2BIbYhGiAntmcqXwuG4XgJGNJ7dbQ%2FcjjgQA5Cq2NYmOPHtMPYmK0ZMb0WTaJMifih39B8EfeukQYBrorJrDboPjlbF85C9NCOyppOwfEpmxTrYjUGOdzKg%2B4GQBVpdyOG6rvlZp8c3qkL6gIxNdHFiXl0jcGeZC1Kk9A8G8DsuTICsPGbSYMgiulSfn6SJyDm9%2BXo7xWZRWP98mh0EcWEoya%2FYClVVa4hqsoAgmhHuHU4WA0%2FnOcsBgSrm1YIfmAUpTbOsEADarwxKDy64GHoQ9sQXds43plEd2bFOz%2BNG7c%2FjDojM3plJMgFJhjgdbGkBTPwQoBAK%2FGBhFfyhxm0fwONto1XtgnC%2FQwqpOFyAY6pgF62102hdVde9vlo%2BFsCbp8i95xukRQjbB%2FHQuOToJOFz3omyiAtJ45dMwlWeYuzIOKvJMs2PI8q8U4ooTPLUz%2F49%2FmYnWBnwYNxgPPn91hQ3FK1%2Frc0ON4hvQd%2Fw4DPJV2CJu3AerDl6k9LOUNoaFHHVzbg0vYWggTpEJeHvtRHnvuOdd13OFWwSXrNZ0E4y3LYOuu5kyU%2FP%2FDDEOnjLTIEAmmpYCC&X-Amz-Signature=6ad9587ed378236e9275e603b00c9eda5850897c1268f61310a8bdd5507a7ea6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

