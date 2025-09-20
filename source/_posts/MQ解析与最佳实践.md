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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGBQYZSO%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIQD%2BpEF0wwQ3oSA%2FV9JfXQJyI96hm%2BGXreR6UubGD8aEXAIga%2BBV14aIdyhexOmOe0qJNTspowhExhLwnsxoy8uy7icqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGEQlEB1MEnYzupe9SrcA%2BQoLpL4lwpdqaLGGwwuodkKUr343N7DAV48jMcXpniI%2BA66iHTBw75ZCFnwWxJHK0pkouodmGbXh5NiPPyJpTdYpircGWU6%2Ba9mLIDZmNXkENyZyfaxZBu88RFdXMI0THQiDIAfPIVF1ZB%2BshITNYp3zzLTcP2SpQLT5s7zCtmzSpJIxqE4wHOKBm00TENpYcWi26yoRBSCeObs3rdMNBNVLT1OEkt9WB0YnCrqCrM%2BPuaQdsijKEWyWfpg%2FEUgzaWn1I7LzYRWjHQ%2FP5qF7xymjANdFoGlNRVJmYHyXu%2F%2BIQVmZ5CA3Q%2Bh8EzTalD6FpDbXGYFiEpBS%2B4dAqeAAfwb7iVo%2FWSn%2BGA2I%2FiCB%2Bd6%2BxijWXjWP0HTPFuEdsgvj%2F0auPiExagtjPZC%2FlfPXtoo1o4M6MFX4a5K0gxlCAZvnXXsY1AM1VlPsadWA4%2B0E6ZXnjyHKFJmv6flA3np3h2whgba1GjsPH6cFVf%2Bv2m01%2Feae4V6rB%2B0mLCrqEtCqLnByBH0dNXVCe5adywDNriC8ThgzxXseBFObq7cTD%2BM0%2BdSnzyXUDaTzE5vOGM44cE7QzkdP0IBqBjzqE0HaMPRpom2aCZthki5kJ7oeilz1moe3zJpkFOnd1GfMNHbu8YGOqUBrAY3LHoybVA1pbZ%2BPHT7NqxuwpCOVh0Y33Y1bTcTeFfrSwiJw0QEAwS1kgwAfniGEBfZTOwmmmQHgBjoqSJWTJMOaSyD4K5H6NhNpW%2F3HyAU7nIlTeoi239LCBQdIx4yP9sRTpP09hxXUOFocRw8jRcBk80yCI0PrZ34BBd8IRtfuSWgYWRoOcnzPFC7BZd4wXyd6HDTgCtGGKI31XdhOkmmnxwv&X-Amz-Signature=2f4373464a0d8eddd2205fe917e32b8ababd5a91aad65dd2e7babac135c178ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

