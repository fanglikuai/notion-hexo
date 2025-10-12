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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7S2YMKA%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T060056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIDuhl3Y7U5jMtXU%2F4zyS3ycHDkZWbWE3bbAzALMussCrAiEAkU7%2F5OfHloEoQhIB0x0ChzjiSUBa65FhZf1iPO44a0Iq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDMvqm9NLdxZS%2F5BxPSrcA%2BNRB4YE94Sn2%2BVEVSYOl4g0fO2rsZqKYQlIzjGvaPwUfxfHzWqYYnlxHxcqngiB2af6OErAsufJAA05Di5htY%2F159p98tgpYypJwIXZYdU4mJbr9Bbk%2BJU9aqTEULkoa7IggUiRAs7O9xaX0O2GVnOtJrSvTskn0t7%2BEvE9iGm%2BhM%2F2j7rfQgeRZPZ7XnpHYflGbqQ8CGGGIpzetJREwVlH3nbkWJJwkO2s4GGea%2F6pgmdV1lmqzXef1B5ZUpRSMF3ElcWBa35TxWjIzUM9TgB%2BaoFkX9dyi1aT8hMjZhh%2Bv2eeof%2FoSQPurXXtRmnFISWXGQKdGACv3Ba73c37joCQ%2B2qCQu%2F9hKvtcqSyHW%2FNdkItFxfuwPdOwb9nBDMCW%2BDY%2Fa8e%2FW9u1VDljnKrD7wrhu27iBkJnKeQbubzTAdayQ1INNOKZvDngvEdDJzaGs88BsiKlqFw25IiEF3dgZ5JlQLa%2FAj9gnWcETE3UfntVkqSfQtUADAty6xV95gqSSemwRAcPIbJW0wZhXr7eN9Oy12d9Z9336WPvwOhrxLldQQCTRsnnme%2FFMn9PUp5quK8rDwUeY%2FdazYgWvFv842dVFp7yw3V5uIK06nk9kUpmQ0PFBjKCQxK3e8UMNnNrMcGOqUB733TALHgNxZAUAsf%2BJzBHQ5l3hn4Z4UAjJ55YOLl2YAT%2BRoSePNfXQ60kj47sYKJjXJ4hcjw4eTW9zTYw5Rr3IRcB0dLN4q6wl2yugD55Ze92NZo1qXttqt%2Bsw1WQ8ceXTx2DcoUfMnuehGRRAcQAeUG93Q9IJE0emWJUYjDxFiB6Gc5AXiQ%2FCnc30WJN3vgyuLz1%2Bl49H4S0ueemVdP4YAN%2Bi8u&X-Amz-Signature=64985327b36de89540633fa44a62059b12dd9f1dc7853d33d4b26be1c7455fe0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

