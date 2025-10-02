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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVL6ZJ3U%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE%2BR9%2F8palb5TmDS8w6jf7jIbTFRQ3Gkbz2ADGHe0%2BHuAiEAwg8kEhI2G5rFnkjX0CAuhi%2BcZwQgzSMNu4YnEyU6ZDkq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDIZVi%2FCyi8WOKdrPQSrcAxfV9lcQktcstPwtWnViEhSU44ipx94VSmQH1up45CZtdzu%2FGj6sW8glJCeyFheqRRGzupmIrsDGvFX6eGb5q7ICAp1QNR8b3dUba%2FAdwlApIliyiyEFK3cyuOLLHlWjAFgU1Ot3Jgt7SN7PiG2TefbROCtTh0rlDtvVUlKMQgpIt%2BScTJM7xtn%2FvuRKMwht91beyQLJrXJv6Fo5QCC8Ft8TzQlO05kpBlnP9d5bHx3DzMbQ16P1sWzNcY3de5KGVtAuTgb3h8dT6W9%2BifKCm39mzJnbbSVEjRuOUpDmWDD3Hbz9cG6h%2B9vvVp33MLtTdvfnLManXK%2F36iu%2FtNjvHZ7u481jDWKuaVHv7YGPUSvrya3nsoKC9Pd67WYJ7p5QnHBw6dkHSsLLAVXEtJPprzS41gXAC6B5tiQyIyVAHTyFh2NlQ49zxsjqrAZy9kGuQCg%2BQNySjPTpps0daHyd9CRBuVEI7VycjWVdRjN0TdWySkJyiubKFThyYRui7T2FrShWvnCAY5wOybyLZG%2FIQ0WZNu8WTI8t8B0U4DlslqagAYCzAmYj2ZIstvqq5X289v%2BV3VeAbtAjc3ldLihzfi9rXIxOmaibuJx%2FN7N3PlMvIr%2FSgnS42qe8aJ7FMPq6%2B8YGOqUBMoMc1hszaacepBd%2BYkG4iSNC5fPJb37jzr2QZazpDdDJabjib9pTKKODhtJEPI%2FBmX2rf39tBBJgMUFz7xqdIwLt%2B8TSZ62NOykKhCi3Hw%2BAzFzYgsQ8GG%2BuckPHwDe15dvHBulKgbNxFoY8DZXlI9iO4DNjQuEk8jR00D%2B1KKOCNX2OOmAiKLmw8B0P3UiKI9GOQ8r3jFNhsasDH8nrO74%2F4rCr&X-Amz-Signature=44d878d0e0c04243f2823a8bb941f84795a9ebc3ec8afaff7adcb78ab12f77ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

