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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466333BVHVL%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIQCSloj9OxPCwH2oh0K2mLgBIbkveT%2BxhOSPcZ%2FphV5ljwIgHHalf3G656pgGGZNschkR1BrvsaAqu9UrgCOJQi77rgqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFzgqEB70xR4HWfKYCrcA6NmY4jCBzTMQcPIMhp15K4HrykxDq8otq0ZYX9PuyG9oo%2B8Val0CmTSHXMZSOs4m5%2BVomWG%2BF4rrR15mNvwL1hBDKGoz1aVMzNcdw%2FztmB7LUSYklrN9iERzwQJYLBHWU5dvXvV5OLAmBKDs%2F%2B%2F8roeyLpD7d7P9gc9nAXnU0r6OEOoKbCeuz6pMMtrJ0TVsD9cD8mVweUACmr3wzaLLo6%2FBSIsgwOjZZm%2B%2Bm0ogrydkc6Np3YpXZ1kai%2Fq%2BYfl7P9q5JBETY5WAgaENh9dAqQYT1y4FJp1k%2BJ0vmYKwIkWW%2B8KsByjC4dL7vJeppsf2yAlj0vuO9gPKeQKn93Ehn8QJBLvIu40p%2FXFxQ0Dq8dasA0cp9BkODcNktsckFqCePMfIuYVy9K16OsWJM51RpTND0Y8zvEhfkGqeoTS2yrujSNpdESm3yzUmLI7FrB%2Ftup%2BUvq34oKPNQg27ejGs2qMr1EtnOnJ%2Fx0A8yXck1z760sOKX9ruVEIewCaxStFTQ45gufrRWpdNvAAEonvSLBhiWv3aQ61cPohsBsJSO6uaEMmJvJVLWHfE0shZ91Ol7hDtP78Ejuc3ehlUs2LXB2b7RQLuak%2BhrGbv1bmXzV7qZdbw4QHye%2BJ56ErMMiNmccGOqUBgxec8daB0jv9BQWuTPYrCEclY4HpzrRY9wVlRQDmW1Ibkr9Ms5Cj759%2FB5I44p6tNhT0jX0PPISvbYp4qICo7syo1b08muglPFYEwPNTKxm08tLLzs3KWXVtMpMoptWp8sL3ZYlxklNk7pDrJM%2F1shbgOX43d1%2Ba5Tc2NBHI%2FEli3gDUFORyIXbV4BPCP8Q4VeeKOe3Y8SdiUXxKGaT70HyoY3Lo&X-Amz-Signature=32a43d7cd792e6fe9f1d86b6a23728722ec837849765d017d38bc4b3adcc9d9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

