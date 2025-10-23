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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2MBRSHO%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCb4OIVOVIceHBCqbxzfpy%2BslRaZ9g3JIeBr%2BHN3sOmwwIgUS1XODNu3%2BLMluO8bXJh6%2BvebF3zIGhB1CmkzrtKdpwq%2FwMIPxAAGgw2Mzc0MjMxODM4MDUiDPD2A4tIPaTxfu%2BNcCrcA2f5%2BVHWJaaXVCehWiP%2BAlAQ%2FuCUfrXrltKSA%2BcXSiOdZ9t15eHjQTZn9E%2BZh%2FdMxqfSuL%2FFTk9DhQMnfNBXlwPcn0DEwev26b8IyJVuYQBH1LYiU2rKFMJrkBJ8BTi4Miz1Hm6ihu%2FdYAwHNww%2BqEaT9tNb39b1m5cEan79FnodD%2BTXZMlLU%2BmlHkQJ%2FA2FWZOD7HoE7BVAGCqO2bPq%2B5EK7CxGRd8ODj1WJnq5iOfKN61N8kJxm793PvY8f7mvuLm7L1isvUPB4jXqHZzRgxlfeZvu9eG%2F2lqPW3djp7XuOTbxMFLq3Pg1LN6XcmEssF8mgk3J%2FTJhCTcvdtX5EU2E9JCE9oXdxlbAw3KXLwAdfrqBvuPfseDE4c%2FeJeMX56jyVHdbqgtG1ZjhjchekJp9GYQRQ%2BzRIV0w0X7g4h4zk58aqNrtLXZJNxnyxJHD6gmchfAX%2BYh1W2la9l%2FKcT%2F6s9%2BPZLkn6naVC0EYRv8blHhwvTOpMINSo5FEFrLYC%2FxuzpsFxg23F53gV80abVaqc9jnB%2BRwpkuFVHO3591v1AhEhyA5XE5sPJdsTwxdrhitD%2Bk4JrJXbHGDC%2BB04Gt4qJctWWvH4R9GOe5l96L8SMX%2FFej%2F54zuW5GFMPT%2F5scGOqUBrlrrATuM2Ne%2BnNE1Uz38JfafUa36pwjSuoBoLabjFuXPQObJApJrOfLIJ8ThSaxbtEPvop4iMu47aqKnvSQnNQEGRgTN9QkW13wEGQKLbgeI4sGnJBAC9275%2FJYG2ZrqDdeZ9jB6OmcIQ65v0TpJ5xEIPMypA%2BXPJDVybn7tp65DH1u2ECCg%2FoXwxCDq%2BhaYmZDhCjK4eyvFLpMS47jnmZOP%2FFpZ&X-Amz-Signature=7ef3d7fbe29a8e7ec73c90da597881c3d63b3228d19b4eb3ccd1392a4196e09a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

