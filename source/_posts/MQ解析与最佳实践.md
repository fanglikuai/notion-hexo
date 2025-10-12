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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNDVUWOR%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQCEzF94%2BbJ1CZbHs2uecEDLB%2BvhEUQ%2BeiepnFgXBaIrwQIgK2PnGuBn48KSrDh1z2aI3UQimlafJaoFAncw10MWpnkq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDIDsifyxpVQe4UG4iircA8ZdTWpJexKfbeKRTVPqQxK%2Be5AtzrGHkAjETSqm61FG6PoSYJFIMsQ9uLmIGh%2BtCUdYLMwlIG%2Bjfu652mAvgoOBPhTbQ5AOCA5ruyqfgQN0PdxRlPmapakFk0%2F0ZKe9Iw6a6AtoJ%2BxCfemER3Qg1KP0d7ZH3kiAF6xVeSoZvMBzDsud4E%2BQC%2Bej8Bvfr1%2FlHEyU9a4DxrUULJcpNPXQz5oVHmJmupLrgemf%2BdEXwulQ%2BlhiAQezGcHgv1HB7gdVwRwHW%2BeJeIRi77K2Svem5tCtme2xOWy%2BhhAiu9ntc%2FqinMXDZoSbhuXxHN0o4pSWNxjb%2Fg8ohT%2FPwLH3eOX%2FhgbsyNz2t0q0UsZlRHEI328kIpToeJnT4dzEWIQCQ5Ih3KVbxQi6Yz%2Ful31QoU%2FZsP2zKc3wQiUU%2FkWtS1r2rL8hrWog%2BLIVQRt0SnE26hkQgwxwBNSxDIPkqT4EL3fIgkVOQ1FjV240Bru%2FDng8p%2FfQbGaQKFWU6LO6TcZQ0bY%2BT5Q7Q%2B8DDNwh3HlvGqJjZi51EHKV1Rwh0YSLEBBmJWlDDo%2F2b5wzQnBrjkQJov%2BquhJmzQ66asdw5G8NR7XZ9%2BFRD5gXojq3IXXzHWfbD%2FBXcOS7gBoYuWzKKV00MJ%2B6rMcGOqUBqQPUC1%2FmmhA2m2K%2FTIJ%2FI621d34zW3oj9gyZ4VMsIFWh3m%2FSXWg6SgLK%2BaHOtJ1K2h4WiQ6m6lN5peTRVwb3kuLKTsSrrM49dFTWKuEv8z7SQWOvtCvGKqFmdGFIGKydWbhlOWWaQkEngjsY%2Bir%2FqNZJ%2BnQKn%2BCZ7pg1KJJPLLciW%2BbSYQAg8kjb7lShJyTvoBEflsxu1U9Vwh2OwSBmiaC8tx1I&X-Amz-Signature=0b4b43c5bffba323d935193c9e65392b2fc3395da0134625c2fd91e362ee6aba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

