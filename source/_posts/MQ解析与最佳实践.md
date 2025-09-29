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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PT6LD5A%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDlY%2Fr10FR9u8w074i3Y08ZD7MwCkAhw1u8r5usR3u%2FRQIgC06UuoAo9pfnYDJfBf8j2HJNt0GjXfNDXcQByi6gT3gqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGjyw7HMQsCM5nCBlCrcAxd0z%2BMbGYoiEhWsU0x78qpntnWltvviOVPa0UMCJWY4SnspOh5WlB%2Br2TI0KyO9LUzx9S8z3Ju7yoDqVMJJLHWd%2F%2BB%2FgkSJmSvkFMRjmbNeLQelN4%2B0WikkL4L%2F39ne8VxsxuuGDF4D4B4avFEQo6oxFJNYWzGbO7XtuTo9nkShba6Cip6cp0BNabGzWoviJFx3nM%2BVnHKjTRtBXzGJzKGgOupq17wJ9FASvuNGTfLknqTgT2Suv2ezoVMn9j3Zr124E2%2F77MjNoKWQ0lH7LjFQwUgMeW7g6pZBxwS1wtqffuvZ%2F%2BDBtZPQjTVq0cmaTm0fEs9k1eQtBKNrtyCwZHesdB6OJFMxSp3%2Bs3%2BiJwdtWFpbYiILFAI4SFOyyA%2B31ozx7TuE9B%2FHdjEiVB9knirGgGIXE0F9LQ6McQUXvMJ1yyot6vyhebLkm5WwUweu6KLi2n5acHxjNk6mP%2B1fWe%2BPKnGGorIYiF2LVvKNK%2BKqDcs9E1cO36BXVmifeFYa3C5v3Nmw%2FnldC68gXbv4KLu8AH704Of%2F6CcnoiE2phXHyh4zvPyZPSTBT4y1LrM9W%2B%2FFmDJmZYywKk6pBDYzwBxEODULM4NS%2F5EsoIEG%2FwciYOadrtz70k%2BIZYNCMOar58YGOqUBdfwsTctfC%2FHlC204FeEW6DijeUV5T1Twl9XiYjvqSWe2pvwCwnTr%2FvTZyHUdMbnDeW5kFlelan252kRQD7DSJzTddjR2AxCLLRy6I7AmvjXF7R5q6CBB9ObZii3HcAOEN0%2BbYHUaEmICCPKFEhyZ3OCEA5pJ3HtTAQSSYbLA9dJsmFbSnsikfvGV4dYll0UMJTR4Jo2kIAUZyCD0Tn0exFXckUV%2B&X-Amz-Signature=e5105a48837cb446f8bb443e13258eb3b6a52f37818fadeb0b17008235eea41b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

