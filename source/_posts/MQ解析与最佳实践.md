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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XB5YAJZM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIDuAVVxXXBlWicaQCkSpkdn3KTrtLd77lC%2FtsX8gEYueAiAuwdYEKneBRnxOSzpfGErWxuTt60C7iMU34Exo9AYZFCqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOyfQrJYftgZCMC8pKtwDfwagpOHDH78MKDhkpN4vgN767d3iTMQ398yQR%2FJGzvnDtPwQedRTr6A5akwfyXIp%2FvRlpTO9G%2Fi36zAXOnDtnbPjw1uSMOOh9UB7xDOJTvq4zCtQs6cpf9%2B4DIANnBCbY6q%2BhZ%2Bf%2B8qd7d879RP4tAaKQfzBoth%2BOYdKwfBkSWLVIrigHWyZs6LCFOPhaGv3wuau1%2BAN8tGJ2bOIyPLO25XFrnDbczwHncJwRccZOTpxhdoaH2Qv4%2BgPtsvZ%2FW8YBvw%2BMLE2sB02egCeqnqebbHShpZItIQ89CJUU0Rho2WGY5LL2eGCWZZQep0xeuVBtm0j54ImwT0i3bnUOtuNaJ2PzazeZqvxemXk5s%2ByUaydQOQ9R5RU1Uo4E%2Bfp1uWUEKiQ1Dul3EaMaG2vnU4rnf6YiKPxTO3AmozSA1S2sxkG0Gvj43e6IKfMQ3kdxzd7Jnvr7fewi2n2ZWLIFHI4E1JSli1uQIm01vsdgdPvv7NyfQY479UPhkR8gCV9P8PI0nbSwX4pAx0o29d8jWsI4kPPY2gNQPO0r4tD4HJX0Ndm0KCbiQVJK3Z0RC8vGMrrAZIgnvQ2BmsHGxIDbxsWHxZXuGTVI7lSlMe9ZRVN800dquPcURwVfvj6weUw3K29yAY6pgECnjRFA2SuAN0%2FrNGAW8175PLgqiqRYYR6CY6I%2BBRIqR3LDU%2F%2FE%2F5Znr2%2FMnQJLjwtb%2BssC2cjkBoWXLpGKR5FcBUSiGWfB1BhzoQE0uWpigmcwDX2kqIqMG6CQdKS5IaeRPiGI1zeWSYf7YYB5oQSLZdz8PRiHlUZlkdq6gOq4cOnM124TzYv%2BlY%2FQco%2FNknrP%2F%2B4pH7n1%2BQ%2Fi3jthJeuLLTb%2FGEP&X-Amz-Signature=cb5a42260330d6c4c3b0864277691195b62cfceb1e7337f073b419b4e3c76e7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

