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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666X6XNV5K%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDrWv6StsEKVow%2Fm2Xg9FaWxdGMO3uGHfnsHNnxqlB4tgIhALkmgw24ytIqn%2Fb1F0BqLsjAcUJHQ%2FWIAF2otJ38RppVKv8DCHQQABoMNjM3NDIzMTgzODA1Igzhu7xhwCklIC6rZqEq3AMyBOzuVzOFwgdTdYPXzQLgRY6%2BNpgmC7LPwuvp5R1fGA%2BX2DRkNh8BNAMMYOvZa33BsHA1rsnPeGZe7wEBzkrCwlTCW6r0Z6GyvVrl1IfxnR24Sb%2BuJY3Lv2MydflM6rPo2%2FQBu7EgKZAXVwaFeBMlofDRe6zr0x41IFyBoxztu4mkptx0td7ZdusvdqDmdtpIcfcUzl60unoAh2nkWGECWsXE7MqN9DrfhRKgRrtYbPcM%2B2w9RpUq1Epq1Y%2FmrY23vxyP9gGLjho8Ol%2F0kZxNWq%2B5t6sPJ06zANA3CvFC0QP80A%2FVMZ6Hy0a2nB5gDrbLBVyVZEJuZFbQQ%2F4evyHiNTjd1aTxs9pHYPLXdLdGKTqQh4u0f18I2qL%2B6oYV%2Fdbp8T8p%2FtriOg2luxGD6wIjW4ZDKXndt8P8DSURbqCyT%2BSfgPROz2Scbbxi2Ff1UZjhfnQsAqnmN5XH27swLaZtw%2BQqRdyOMZomV6rrgl%2B07vPQv%2BB%2FbRe4NiPKfOuxTEpLx6x4qP%2Bxg42Doit%2BhHLJZLutliEk3hehTttuS6UHsVrxU%2BmV5eRz4aXk0ikFVvkSMJA4%2FMQiBRsgGzWN%2Fg0Xwrgy8jsjikto3SUGs2hkRKVaPZpFiRPr6Wd7iTC4vdTGBjqkAcb4uOqQ%2BsGiS6YtMXm8MD%2B%2B2LR05NgZuUoxU1zJpiz7Qbq%2FjzRSjAu%2FqaQfwWhtVEj1K3O5PD%2FZ%2BaY8LkYmbPZN5t6k6gLMRcsjAndFukegNKpbfG1%2FzNC9Z4hRezCUziincjB3ZcvaEqzBZXuaW6q8baJC799YfWuQfefYs2exLfziEgt3FxUhTxAeS2d1iLWBho4sVNmb2JgbaviwV2sECw2R&X-Amz-Signature=3827d46226fb45254f0bb3cf0eb182c1c1369ed23e84ab727b3834d065b7e36e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

