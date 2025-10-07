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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7B7ILP2%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T150105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJHMEUCIB68Z9jlxH%2BnCbCoaEtBL2GctrKiDfumKUKh0%2Fqvd8PpAiEAvQi5vsCMM7H%2BotEYXHx8VNn67KfNBrynaeHIQR%2BbiI4qiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDn0EaKNOvkLOP7O4yrcA1rvA8OPzXm6ch3jWmgsNtAqzOsSfLFDl%2BmVIV4uIjRa1sEHszhi5Yskbho9XxxGpsI%2BCIFl%2FRcKnRC5wn2nX34O6I9OZeVfIdsGtyKbTa7t7gLvJLi5C0TYRUlfBrBJYOa6L6xaidXn3m1O0owBQBrv8FksAceuT2oJCCAmJWLxpzz8upUXdJQ%2B1qH4ZZqZhIFy9Pr%2BOop4T8C9rpihcyAbG2sPvCsNUn4tb0PusmpW3SEU65kBsjTvSkccauw2PsNHUXt4MZkSxyVMulHdeOOuqCPiTBStPKjoJrhVfhe2fcYzDKKuL4z%2BLK%2Bl1qWNDk7u2pTHHtWucW6U%2FXGVVuUdCp9lDKuHnaIUBtAaE72dfs2y1n%2BFPWiCB4cgFU96gcrG%2BJhFxPlf%2B4zPSJpDa23BZ0eyqvuqQ%2FdBfu8ZfFP%2FM2I5cDwtU5boZzSLJpivEvpi7Kysp6KTHFmimPHhpUsXwUya%2Fm6bsMN4BB2pVJI5CZBNyDZCoVyl5%2B5DWaIXp%2FkrFNmowPd4OqGVI5HpeBxImAGdxb8nrnJo2%2FeF046uJLYE8kp6S%2BbDuVXlksUQuySZ7bkBViwWKZSH1YnUPoYR4SPNvUEd8x%2FcWPHddKTKFE90HdnHMkCYlCjxMOe%2FlMcGOqUBbEmfdkD8pa1fJLBWq5ZLnojjGs%2Bd6OknhyiDYyZrJ2eRTVNJ9mIWidgyrES2RvBtoKXnk4MqDKadQsjXh2%2Fj83tKp6DG3jqrsWyEDMyXhQFpKX9sJan6z8tTWWPU0tU7mibcOsg95lsMpnYhQyWM11u05A23VOD71ZAkX0Xp9UITPp2i4e6oTATTrynY%2BvOhOOYbrCTrs8LlxenONg%2FZvxr08L5y&X-Amz-Signature=9adf53f09484bccd61fe21de4d8931cff6afbf3aa5f0530f434cd3da0018f72c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

