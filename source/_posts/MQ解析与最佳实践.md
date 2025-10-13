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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664H5DZRHN%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T140108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDsUe4Wq5L3KNseRALcl8mx7%2B6GAroBVRtETM98x9%2B8BAIgWM03u9k0fObZveUprtRiCQvFvXpI7ZrQetHQZcCzBZ8q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDJsRuApzec8547zWPCrcA3%2Bj%2BenySulUsaLvSkmT29IVJ5OEI8MWMr9csG99g91CW8cXW1zsuRjMB8izUvkqHL9Bg2boJR7PNQ9b8yg05q3RBfbl3QkmT8v5s82F%2Bd02QvOj1UJHc1IQm3cW%2F05Kl1r0lVQFxWq3Rb4jTBzd%2FsF6dRvFORgTJ3x%2BaDxXOO8cMvR9AblAMF%2FmDAE44f8xIYuod2C7VU8IrIdQXC8EcQtltwPoL7n2cXRSPOou2EmXG%2F%2BAb2556doH5qA9LLt6yDWa0UmGWn4hlIpGW8hRD%2BPDQLPeMmtbEZnSm6p6dBF6GU1g94gGFQpcO%2BDLqe7maixvbZHTwXh0%2FZ8K8vmlIpxdkHmyecAa32k6IPzcU7fYiJxQYccXpck8KYNEKKtWjHwyTMEHycE2oTVj26J29gTo6blJVY%2Bx0YgojqWOPOI9feefd4TgZlUvDk%2FwJPodZgua4ZFfEAerz8E3UGCWDgMDERra8huH4fONjprTg8KvnuWpVg5UvM7Yjc0FGCkAFH5gqoUob%2FHtZzQ%2FeMWjj2EhXMsBv7lyA4IMLa%2FdIIMhLLDm8t2RGfABzMI0L6Lu4aO9mJ%2FgWX13GVQJdqp1HFhuNuzgtdPxXnOBPxQsgOF8nRyqd7VdHPbxr%2FcUMJDvs8cGOqUBjEoby3Z5da0syqbEu%2FsYZ09%2B2SpLwgBH10YRUYFWJi6%2BOPGo6Ggz1MLbDZ7GnyS6GbwdlEbFc8qo0I4P07iSD9BwJt%2FFaLXN9m1jiaixxbgRWgq%2Bx9o0SzUCm9teD%2Fmjd0Dn55%2BWA3e5DOmYeRjheRqm8oXQK2JS3i9H29kNLF8PCm0nq4gCwO9RsqN6jVaY40a2YVKAwH2iDQtpKMWUQQpDgJ5s&X-Amz-Signature=0f33c7a5abb9da2efb56a0d00373195ee334465b212ec76b31e6838667e6362b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

