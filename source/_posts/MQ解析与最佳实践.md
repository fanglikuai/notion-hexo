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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKYTE4HI%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICclSKL4N11JHLvnzHRMjmVh40MLAb8%2BBnXW8kF5Dcn%2FAiEAyZETVHhz6AJR48ZFj2rz%2BzMS3WeyxPkigxJq%2F9hwSeUq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDJqzuXT30WYX20gpdSrcA8wVL%2F9G8mnwKJusSEzuCm6OkdI8BSL%2BYl5FQOVo491uSfkndg8Mkr4%2FKPikCUbvfHrmYYyMD4FW0dBbX33hsLD12DOD9Wea3M0ac1Hx0KmA%2BtMiUAmPX9rq1tC0RlZ5B7bVSWUVyWaXQ6iSa54REmSJQ1rxqxmK1fAVBssb1TJ6BOvydcCymy2kQ94fhkrJcYkaM0zcAbcqM%2B7wn1FxutScArKUWauoLjVoKbmwAzpi8jaXrKVdJfrWTAp%2FgHcg1zXdKmFwkkw7jgmBQgHf31PAE9Z7CqZW0DSidsWdIAuO%2Bkyqw%2BXaaIcEonBbgFu1LyxhP7rYYeqCZ%2FX6XH4yYTwibfRC5CNc3LMP5BwEE7CMLht3Qe6uxihElHyUTfDII8lw1XpNiEvzNoGo5IKTrd0xSOCGxw%2Bg60LD1XzhawL3tsP5riZJvGL187bUa4YeTIM%2BdqL2NldUi7PKdfecXBsWrq%2Fn48sCBN4KUqEK39FmFrVfH0vTwQgyoMwhVBbFyu%2BPdqaeBc4enE8%2Fpd9%2BAfEblyLqOoCe3s%2Btf1VOL9nh8zuu5TmydTEzlIoE1oggRkJg2K5E5ZGAczxNjf97PO4ufAyycAjCpduRMg542pwyfctOUqZTbQ7UsIDzMNiJsMcGOqUBZzPfltrAJMvq%2FDe39SeytgEvHpr5hTGUryUeNq3TR1o%2FNswr%2Bt8r%2BDe3wdiU3gFfeRLGWDyhhtpq3WSlxD6BYzFUhA1bEvIwhgVoZM0GQ6GKxIrB36Ot7d0ktVHftVdojUT2QlCamdOpw7Lbzexg884npY4wZZZ2DNasPkTyj4kC1FbAZve1pF6rtJJD7NSQwJ2ewIehJm7q1Cye8yBvAZhFst9N&X-Amz-Signature=d9db78684089a253dbd7b3f618a6f6e37cab46b4e0c6cb0abb34ff3e65fd1b07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

