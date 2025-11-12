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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646DY47KP%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T140039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDRkgrzk8JV5lJ5MFfSzQ30Q4nKax%2BK57YdcsAArm4FyAIgfN7jBY6sFV7TuiaMGTcZ9RJhCw73cZ1QiE6Gcf0A%2BDkq%2FwMINhAAGgw2Mzc0MjMxODM4MDUiDH7CQ7rv9fyZLy7oDyrcA%2BJvbZ2%2F61aEWZx34%2F1qvIAphOXSW1B0p5dN%2FDH7d1WlpD5WWz7LiYcoCvswQkCY4pnhjMkMVn5VotUP9jsvWpfY3nsOYFbS1aP%2B6my8gLRs3d2zpLqH1XlW6i%2BX3JwR0gK%2F0LwLVz3ghPH%2BDMV1KylGfFXPrmjZHsMrFF34WsM5Rmoxb2k2BNjtVQCE%2FvKkGQL6UtiIlCU%2FmZksqlSMp%2By%2FR6SRGSON%2FU%2F9vA5g5DiHCg%2BjOf6YGeZl9dyG%2F%2FK6ck0l71yu0UABaJCnOc%2FEeGahjCvkwD%2FR0Bwl7dqBHlwLkDDvTMyv7i%2F26DDvfmyOG7atLRSeVCKzI%2F1M1z%2F%2F%2BqY24zD5wg0XWV20Hpyi9kK7mWX6puB2Z2T8oOQUo3M%2F0PF8oKG1YvpaksvPo1Jv6om%2BdlFIQDUkjETHHahPN30qFyKKJLcreFdbThnaOcgtXNjWfMev3uxUHG0UQaySsySDwEUW7p2dpP%2F%2FEimZYbJS6j6DmJlEX5bl%2BeVRWF3%2FjaS0rQd2e9d5laDNxCR97hU%2F9oElKckD1z%2FXztyuU1Arog3tNVWghJhb%2BXm%2FvHssqKYLqe1QKm%2BB9YxbbnqrhSGkOfb1j1SwUt9Lcmryn8HE6D53Mbl4ouiNODgmMIeC0sgGOqUBYEu1ErMclZO4ZDm4%2BEAb%2FUnQawlc3MrkF25%2FqQmN6SgFYbLGIX96bG2PaSuSfKyFF1uhJdTTM1%2FxA6xIvH6a1HCOTnE8ZZp%2FM%2FEdMExUdCc4KHNSAxGt6S5hX6rdand3pvOHx2W5aO2rUmiF7JBpNE8xZXi406z1Ob4rTEtJvsMEwTe%2BqhZ2mJOOnFdr5USOOoAxgHhLjqyFjfa6uYiZmQI3GV2H&X-Amz-Signature=9607cfe9c962367a52c9a5b4e1c7e33b26b6822c248bc74596064a84c463eb1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

