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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6A6ESP7%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNQu4T7sufa14wiLhBugW7vVBLP9iaejwNTkMOc9X15QIgVWSaBylFLrFu%2Fj9k7YiTjzvAJ9B%2Fx0h%2FY2UHmIUFYAAq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDO9KhYTTPNexRPG3ZCrcA3Og1UJUw6sbUoHxVIy6FABVwvjRoQ6spa5c38za%2Bvy7mYms8%2FO3pj%2BW1u8GsHokRwWhZnFd8LH1sgYygdomFJptdSoEMGC8TWeBaDnkQS7fxBfpechBhij%2Bf%2B3sLf4E2zA36%2BKcF%2Bw9gWJ7PQLK8spmBsiBBFV2U6qhiWY8FCoxAHsVl6%2BbCD%2FCaY9mPLuW5CYncHO6jKnPTP66VJvBzdKxb40%2FvEAqiUwnyjlNDIVr5mSiUBwovNStC%2F6XS6kgbqPLU6JzV9CTUyyFij1AIkSjkLgBiPBz94CTq95SrKIzDzLHivhuIfXXtponcIkF%2FAUhehpwc%2FEjUqdLgIIBSFCNpDoN0m3L02mAlJGRetJcxaLRUqWJ%2BGdQk0z6F3wOOGSiENtJ8AFS7lMqwHoQsxjo9GqNQwuPalMm6EibnsOVBx9ySW896A8bvD74vSIqOSr7Iwa8IJGWf5uoCf2zJCh4MAqxefvJjIdyqIwJeemUwhki6eEyrrwE5Sur6WHA4hpju%2FCA2HGQ6CU39%2FbKle%2BB5Es%2FIrGCfIiTTI2J7N%2FKvx1nc5pScF%2BxYJMEQyaR0Orbww4diTgGXv78fd5h77gMKc83rLwazAjKJRg66e7rGvy1Oi6mh9NkRw0BMKGNgccGOqUBdGMuwHOhnmi%2Fq8mrY7u3JzmS%2B%2FjgTpF5fiD46%2FVEypCHsT%2B%2FLT5q1qRusAhuDRynKhnoyZNGB0Uc%2FyNNOENtGmGdwLBpe4hsADQ%2Fm59lsyB9zuaQG0lB3azpE5f75Ul2p4SzivK0yi39PcgOPk6BUxllPO0dn2OFpEOdBZj8%2BIemQu%2BGavYS4drNmjVdq%2BkfryiGjPdOa%2B0VMR8ethyfHPr6MOI7&X-Amz-Signature=aef7e73fa73a65024729d9672c1b94d31263d7aa6f9a974e2127af46fb48c1e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

