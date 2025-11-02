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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7SEWTE2%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T120049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQDDKS6nHb2JwmdIskAprk4X02ntLta7zQGH85iT%2FeFUBAIgDFaQh2JAdcr7t6u%2FINVARwMPM75g9oycYZkDQDSx2A4q%2FwMIRBAAGgw2Mzc0MjMxODM4MDUiDPnPJRK1bIKc4LLxRSrcA%2FS65nFjSSvcHhydE0h3TAwmhLbDlQ3i8umltgrmcRsyQ5m4HfLJsl3wRECbrVWlw%2BCKpvW%2Ba4i3ov%2BZhiT9LTO4rmCaxAenO160RpAO0mQKm488H4nkDkWnmJ3Z3pS8Y4GaNRMfItxqBREn%2FxKmkozZGYpWtbiIkxRJ3iGkB%2F5WqpQnzk%2B0xtB77PJ8ipQF9Y7RqsLHa5Qh7stTcJLKZtRkjMqP6iWtqdxYmZ4Omm0CHDhKXOBXpsA3sELZWA8dOZ9syTHHpyKJxX2pBFaXDyTJTvFO3RR%2BsYzhjCW7%2Fq1fcG%2FIajH4q3I7g5an1CzWOoBa1EbjupMGyVfbz%2Bb5CPo2dLDvfXwJYQegzJpdTjg9sVdQ2l67BkBrXmhVgVEmw1TqlcO%2Ft4SOTw3XHOLys6izJVjtgigtq4Ptaa9Fvxqlp%2BK0%2FnE8s%2FeTNc9S5sDfos9wlzhwkJP6cDcWeFcgdSwZGesPz0o1%2BUZ7hg5kpNC3dYz%2F4Fu8kApnPsWF4LLTteHTxZlp35vEX%2FUZ%2B6Szh4w6tjnjBMzhVrZJLsKCMuFyqGPzF85tlWil1Jy1M7ELznDWQm3OG5D0ruXOxMWjkoez3awBOEdGrfBsqghrC%2FfPJxLjaVLk%2BgPyzh7LMKj6nMgGOqUBw%2F4jwMlHylYzJCBd8pIcEOO3n0OyzsR9sqlK%2BeYSaIyZ9ikp2PXuiWZHO7fXN%2FybE%2FvzpREUOgQ787kgVaoIUhU048rRJjDg963SDh6gxqylzUdFQ%2B6ZSHsduD6McLOj6qIiYetTwP%2FRG3fn%2ByPWy%2FMTKC6C4Cyu1RIn4LVuiUklIjc1zMYfq8MA0yimqYBoEdyjWEm%2Fy4YvzwvZgEYnDXRNcihm&X-Amz-Signature=7556c17e01346235216c5e38902861fa5f0d63175ae00743805a3aa33892aa68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

