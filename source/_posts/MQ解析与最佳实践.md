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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUWNDMXZ%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIGcalp7HaW9boQCXr5XYv7fAccieDtgu0rxBo7%2Bz2gyTAiBI7bifevhq68Lm4ma%2FRAWWHEh1dZmFiYfzhfIW2%2BqlkyqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BvtGXYObtJY%2Bk8oHKtwDSzpkwYoSpWrhRo0RgTPlEZnlCHOvP%2F4f91I2x3IocDZmw6wXkRcjkHRiIVaK3oJGqXeel1FJTrvVyfY7HnIvuq6kMLUPOTp9MlB%2FjC20p1%2FQGFezebIi7tRsNxhQAMPZp9m5ARDR2yRcOtMYTXmyJdG0zyYQvCpobMN9GTb5szvK6A9WVSSpYAkSSDNax9a%2BTKQMmGBIxN1qBPjq5TamDVTSbNoWAnn7VDYEhgHzZItb5ST0Kz9owscnB9bx3EToeL%2BRNzypAVmUh9teiOiM7coC4yA2rW3OWl82NjRsM4%2FnSoZ4UdyJHkbKAPPmyKCbrpB8r%2FnVF4NGKmPmO3eTp0BHGaG7sKxSRPgOSdu59v%2BK%2Fxx4xeMV1ou0yBc%2BIym392u6vFD9fjidnFGF27V5uMbGxRF5MzX%2Bg%2BGo7R7yDIy%2FnGcu%2BmLJn9BqQHtFdqn%2FdpkqX0DBp7bkSNLgsluxr1LnH%2BEQNnjRKv%2BHO39q24ZFYzg%2FZ2ZUTWDe3qacr2TPy14YdmzZhrw2aqiBp9uVF9wvRzYYePXjWqVRv%2FgnADm8CCL%2FgN9%2FD%2FxJZ3V8DVuCSg%2BKUJ2%2FIQyBTdhDI4EQyKJj5ANbSRT2bPkBvuPScmkMwsDlNcSZ3QskUFMwuu2QyAY6pgHpo7OqRy0uyDWVd5NmLoeEG7ADoQcU2xoiGEYroeh1GCdWfJNS4yOQZRnSyTPfTtWX0q%2FKFJ%2FpWXKN3Dd92cFM6VGMyVNfOkEQAHh6EN4Xv49qhSRONap%2BP4%2BFcr4mIkrFC%2BhJvTQumgyfF6bB5hTKCRu%2F%2F%2BjdafsjaAgp%2Bol33hKW4X%2FIba%2BfpIZCsUnoFWR5UKifbF02vl6MxV%2BiMxfuF8b%2FXYGh&X-Amz-Signature=7219f6bf0f9331e77c05eab2dc65126c6d1f0dea1ed973bc0fc58faf7efb4b04&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

