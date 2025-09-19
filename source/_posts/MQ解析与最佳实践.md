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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQOLKRDU%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T140043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQDNZr4Ft9NheQtiNYAUTGRl%2BFcZ4AuG9h%2BWmyHTnjAbigIgfF5MmtdkubUsggHSi9J5XZZ5hk9idipuWC8jLbu6ViIqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJT9odh0Z4t41NR4uSrcA9ja%2BnnNuKvZRGejlOgr6J1GTjHnE6%2BJyw%2F4xYWZQbB%2FQYDADVZ9wbS6sUwGULKjYWVFipz9gmcKKnQVUM1iHYiWqqLi1rUZd02FwYuK%2FSJc5A8ZJy7EUwdzf%2FNsWAx80c%2FtbIwq2zokifrsRM3rHrLHaMF2SVfozR8KgMn8ykxUoX%2FC%2By0s9gNEuiXzW0we3Dp78uFBjOiILDfeMDelklVwI5L4EoZkHsSsxztcze2mcs4oBHx4AcDjfOxSqgJxS1SEN%2B8il0ayPkKxo69mbdTNbQ8mGdcp7%2FkA0rg%2FwOS5R00cKRQcXj2IBNi9MiYDjfGxgJ%2B3pBVMbA09QlvQpKSq0rG10VXRFt81sQvaSUrLOHvIQzHCB9%2FbFIdrW%2FQQifv3m%2FtHqY38fyuJOiSs6jN13zppR7yJ7kKeMhvv1Pxzje87oGmyg2MH%2B%2BOfuWmxZiaf57NnAzJGDsRiRbZcgOd8z6tQiyDPJ4b%2BS%2BQYfT3wnJnAr9%2FB%2FwwHUuNGFc9i9vXrNyJ4TL%2FQ1q1li64Lolox7bBtbe9Vu7IYKyLXXh4zVzpxrm4C6vmndzM5oQhmpfHHu5lKaB3FZj97OISW4Sf%2Bv3d%2FbDg1RlTn6tpRnP6xDP1i%2FrPOsOTDZsxOMJzjtMYGOqUBMshz0itXesrwYhB5t6UcCWzGVavRLNdV57NWtpAmzMCJ0UPDcr9SGUCGcQpD%2FuohuLmf55%2Fr%2FIlF6tBhFpM%2BwTDE%2Ft47r%2FYM1RLoMOLujcJTgBjkE7SpgYtNCsAFepQ2w2EO74llqlTKGGKV%2BxnAw%2Bx07wfy5gllYqJrVgP3dbv2I2O%2BD1fP%2FSNTUBDPGkrkY%2FKmVqjRzJj3GJwwjVePlE%2FJRBwY&X-Amz-Signature=d44a90a7b40104f87e4d478c1b916586883ad3240345f0e147fab1f42b3c1d3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

