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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGKB4ISI%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T090044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIDAxWNiHJwQvWmpskfhdwSQLrOLim%2FxOOAv8luLMZo75AiAyZ8kQQJGPfxJzVAxg8beC4doWi7%2Bkv1F0WwGGj%2F1H5ir%2FAwgZEAAaDDYzNzQyMzE4MzgwNSIMi4ftxkzirSOH6LlBKtwDZhuqB5ARoh9y4e1SvbgJjBXHfU7Evjcf73Pr%2BBqaTRifrwAHxW7qyvprmS4Ctz3ogVUSQw4bdvYwjFk7g4fs96ejeDl%2FMLW5BcnZr%2FyLkW0emFTJB3ijb4xfzJdBueGSy5zqkbSX0B8J7Ls8dZV7jP55%2BrQw0BC0%2BcAWTdgExCMnQh9n0G%2FX0T1kZ%2F6a0WAubrSwA2ED7hSQxStpMRRzcCeD%2B%2FT%2BiCSkUCYsHDaNfsk0xYz5dRP54fDyFC70rrGB3mxNV42sxgIvsM5BUzIbKO4gN5WWHZ2CZaSHZSsHArCf6O6A%2BfjjOGYxS4M7sPc%2BYAY4CESVj4O0jC1CRZIGdpHRd7feGIgu10VLWXZWhfbosrj2ej%2BvQById29dDZPWlkFHS4nzuwR%2FE2KdfSny8HPjAtgTiPQ8Nob%2BXHNeDIRcMoUN%2BT%2FGM8EuanzCdHcHKo4i5JR%2F9%2BMRtJBbTkFewTXZNZnF%2BtPoW%2BPL3xIEV5WWbMU8yzzMuTBWD6nvx3339A4PgAS0vN442Uvrw2VEzR6p5eFfzCK8NMpVQXGQEVT7ZNVqlej3mNvddnxeNGYFtvvbWxjwxmLgI6Hw8dnNil2oZUx2oAZDWuM6KNatRFUIa8l8%2FfNL%2FLeA3ysw393LyAY6pgHTrG3zCPv7FQapdec1%2FpdIjczr6%2BqpGfedLUnPbtjEa88BYJV9qqA1xx7vfYhirHvlQHPRVM0xmCMameL1mapdnmIugCdCC22R0T5tp7miRJZz%2BUlIS1bPQ5ub%2Fs9F2LWr%2FRBmAVEFJVfjjUAYWAysWe37jOj28xaNnl%2F9l%2B9UluXI4ie6ZGDKE%2BZFHl%2B9YCn2Fi2SazhC7ZyfuTeuRpYQedRJeLcD&X-Amz-Signature=b73c57f744a2f124a7af99c483c035b0077ca146b3d04e086ee05a40e2efc00e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

