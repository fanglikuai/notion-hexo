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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624SGGBZQ%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHW2gMmLa2uyo7FbIHnzFfn1R9aAOiQENWilVTsDgr23AiBZik9JaHOkBnwWZDl9Txexy8vXuQqh%2F1rYq9tvtNkNJSr%2FAwhpEAAaDDYzNzQyMzE4MzgwNSIMuwcXRq6Whgs41m4GKtwDIRpRmYCmp%2FG7mFvB9AO38D3HDYmQ7f77l77zHdJ01HKYjbpNd93CsR9qeIOgBCyg1S%2Fe%2F1tlv2dqbWdWr%2F3JmOXT59gNfHTT%2BQ2%2FiyAH911VSOzSkg04yHOhWPg09lln0XaI9GPt%2B%2FeuMJUH8XEoHdQkcsp2ZxJD21oWIKfO2OMB0fqdin5%2FLw7N9Gyw7xTF46cP8gWlVcrAS2pFyxXxTfutUda2Ac9S8VznFOvWzNKSqPIYKZEEFbUNRFf2GL8OYdts6NWOBBdNUeHYUnDhS%2BFAYfOuVfj1wQNy2iLAdRIMoLrD0s9OJ00J9NXWj6jMvHTN13vvB8Tp17oVD4Pq7aw6HEoJ4Wt5PiIHBz8Tz%2B9qfA0bviwJt1rPmZx6o5yZx3ghDBc6ylRPcHEl87uJa0zjKS98kYr38iI8PY5TIXXXVldDgTnIxmTqZu7s%2BBnE0aibpVo4KkUjvbYU%2B1KjW2UfP9rT5F5vOpsZGfPg572rhjNfkr2kE3FzEM02QAKYxttD472gk6%2B3Sar3oankuhYJdXGji7gphcCdl0%2BfKGCkHIGZzfCyWM33HCrgwrXtgHivlM9ZbfmxFwgci6llZMIGkfXPcenYZHnRzF6%2BhHtnIM%2FIyPOdfzE02tIwheGGxwY6pgEPaWsT9v7tVYgouS7KoiPrgcx2oihcgqr0BDPaHaAYx0XZWjk3TvAbPFQ%2FGeMpJRiys5qVJTwspk325oHUsWXLT529koKxHawjK8jBVksRDSJa6y77vLkYWsLTqeR%2B4aGxufmdxsCB%2BAl2bSbmCwq0YUohZEocMohg5kxYgIJEyZD%2F1YR6Qy%2FNk43xMx9W2JcH3cpRhSr%2BNbhFVCOr3h5eyynr5dHK&X-Amz-Signature=63005ba9a00a865865017a08001b5a1fc1d2955cb8a37abe9542165aa1bbf8d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

