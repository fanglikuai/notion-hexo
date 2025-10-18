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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RY4AGNCH%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T150057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJIMEYCIQDM0cKgagPlwgdD41DUAhn7Xv%2FyCWKRinlNR1dmW3yyqgIhAM5EdB%2BiOiv8U5I7cyDIkQqKzwG07DA%2BBUWRjVujis8cKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyBVKpvQmScEc5GLdoq3AMukrcLy%2Ff%2BZNHJt9EX9Wgml3T7y6jKNFJUmM9LZgBZ4mvsO6bYKOlsSuy9r%2Bg8Y4ZIdaO6oQjFWsLEbcQzVijqmLgApZXoMokyG%2F2L%2B%2BaeKKLPyQvQGgXplyB2Lembe%2BgkQu4WABmWCsGiPG9%2BGZ%2BaOQ%2BhZb03RLYRRDB79vxlnHTIhgsY06GN8D648%2FEUl1t23ownLaQTHO1iHHhecy436dCHTRwc3zFX6ieIy7WL4czMJKYjALBZ45mNVW9jMshL6ksjULi0aUWIFdxfTxlyGr%2BLDkKGNcsvafu6SmZnfVQwZxR8p8xB43xmaLBlMw9HpHbD0Wm3%2FjAmop1EaTPuZe329O%2BA52BjCTctwferj647%2FJ%2FfzfPoMaxE4RggN2PWRfpUkPXH1PHM3OLYVCc494eUkZni5F%2F7OiuR5As1kqKkfJPvciFXu7LgyVfB72PZx96Hd9LSI02rCKW3K%2FvG4uUHve4nNz6wFX9tOHz3dwHXiaYkfxz38E2ua5bSEmy7XfSqXseUuY4yC1p2K9o0rHg7p6iEcUXZ0IoecDGqWtY1c7ffrjfyV2cwClA1jx1rN8aadzHJL9pt8uEg1wmVBT4nIcZLrxZ6oW8kMAmYjar8bkNeyM3%2FkqL02zDQis7HBjqkAW3fwES0DmHqmqk90fQu5SkcXfHIAF%2B4dg9Tno5WDIlnq7PURullB1V%2FOp7UFuupejd4xdNFAjRqkAU66vzjnhDKCglRZpNShm2SqAFPoKV23bWbPzn4Yh5Ped0dXFVfZi71sLWDV7YR9GFseemyJ2pN1w9VcAsTniTX1FR%2BZ5QP5SucwUOpeYkyDUy3hhpf6Pz1mEussvmQ7EaFs%2BY7iVLxlqs6&X-Amz-Signature=7207e7ffc911046dc77538432f398c25c8355a7afe6bc53f0dac3e266ec5b532&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

