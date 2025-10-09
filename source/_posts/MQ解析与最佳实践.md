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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JDGUL3I%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T180159Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIB54f2Vd5oKIXXENN3SqTmnZxAeGzcMcfSTVCiuK%2FHYWAiEA7zXCaOoWnr4oYJN6eKx3qOkykz5N6JW7eAsabfj9ivwqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIGapU3aO1nSBqMF5CrcA1WAk0mVn6DkvEu%2B4RjvRq0AUR3hOJQZOnZ12xqr1ZFt2QLWRrHmktHPDZ1OqLcU%2BKTBOaMmWSN1yNLz3fTmGH2ztPiBr2LTxwoC7WtdPih3gHEcY%2FkuX3fhKn6ZK%2FC07%2FBsNWXws5fdflZW9l7WXZnf3L3Ecs7LpgVD6VLJ%2FB6oybm0vf20xaD%2FaAmsg5yU90RC5HhkhCD0DnS%2BfaPscYzC2KQ6jI0owIRzEpDc9GMQSZaxxKGe359cZq4h2yd%2Fq0WoSMdqpyO7K10B6CWtxicbw5WkAItll%2B1fs6lgFBuAdICadzt99miWtdEjBWBRmplvp1%2FwOexCF0%2F7J3OXE%2BZnzA%2FsqpEtF%2FPEXCVFm9m83EwEYTHCPJ1dH4VQ5qsUzEfuDLl%2FFlnw1CJkQbwLCkH1FC30pqkYM30lGMUHNNZOppg8kxCxsman6xAVsnbYqtSv1VH%2FP9RkLuu24U9Zf2266vLlC%2BO%2BbFoTRnWedaGLW4BFrCUUJe4x6w5hMnDEJwaS2vtqIWtrnczjEW%2FkNVhiafvJQnRcFNHy5cr4B1ygXJoZYDRe%2FLy4jnGYqDBoKfnTOjOd0KevDJvgWadWvWqNSux33K0qdhIbSoK2wE8%2Frp8fMg01qM6pEAxiMI3in8cGOqUB7h2kR8Sk4x8d4GiAKPN0hqv2uyJB0Kbb05ks2STuUjNIW1Z8BLcPoxFHgw735BVQLmmcWFL3wIibe%2Fh2ze8lEe%2FkBskFAx4LjwKum0hXtd0WMqBJoA%2BOKLOk6n95JRvULbFyJ1rrQDVOHYyxXxzEBqZVqeqrWcaYb4Bkm5wURJxujocoPA4epp%2B7RgEyPq9YhNlVnyB2ct31whZU4cIk46VCZPgQ&X-Amz-Signature=8e712d10d7e09d4d303ed6827d8a3309da420c4dbf835a0c359356e68c08b5e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

