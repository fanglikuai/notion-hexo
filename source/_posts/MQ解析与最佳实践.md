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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRXUVK57%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICe4qlfILdN9px4msIaUiJCdW8Kte1yZ7EG3p9sy2k8jAiBzjYIDAefD%2BLZ0Te%2BCqaXniIuDOmbHiRiYLfAR0dk%2F7CqIBAj%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQ6sNvRqTURCPofeAKtwDks%2BbkVQ29pYn9QRwg0bgsnNSmyL4tDPqPbP8dDjAHu0Y8Oi4L1akaDz%2BRYN%2B8uvPQ3lV7e7O8nN02KpRSrNMSuo89%2BUevmTxZJj6llvnKZhckZF26hNnKLgLGm8%2BfsK5blEWHX5BRssi%2BQNZM5nxslv2J3D%2BepvxEBd8hnIdGoqB9NDo%2BxvBz0YRjWHncaeUq0oWcrrK3VJfHm8uUgbIRAtReOT9QAkaLt4qjM7pV4oDbFX2VIGyjic8DLjoZtiwZBTZysdWrGa2QmG47SQgUsLh19sfxz1hW39hIARW%2BjQK91IKMIO4%2F%2FqhkcmxXkSwS4yjO5TCTqQyo40WhHKyDPDGPBDzrb59KLttpbiKUyBqaKm9i19V7qUtNHTHtjiiQkSOoanFw2VOCB46w4Y6wA5b%2BUN8p%2BJUDh8p23JjetrNgZY2dE%2FEjsEszVZzSdwAxtdNc5cEhYunsJWPalv6tg9UC4NG6gG%2FP6y1twff213xhJho03gbdUMSgX%2BTSkGCF3A2j7pMl1UfyZ28ZLXyQT9rZsardZ8fUVCBTWu2vphjCmDHamMVlHzyl19izLsyZwgYcYP%2Fkv12ESnGKnDwEDYTXbCIZsG3KvbtsxZ5Qs24FObFxyrh6l5Ab5wwif%2B9xgY6pgFnqd%2BwGz5Ip%2BWEXUJbmJI0oFzFEKGur8wq8uZw6bbg3GYGak6Tuuez6KJRfcbGv64ZqVzR2c9ugBM8mlGdF1zkc%2BrDV%2Fp9z%2BqiVDRcc0Gd8NZljcuBXaZpfzypeie7aCqFUd4Ohn1a8UJ606%2BOpOd811Mcp1mFCjD6UjpuZ0FCDbsqJo3SprqqnQbKbsINf5EnXm8tCs9ZWoGSvACcy6WQ3YKRvP%2B2&X-Amz-Signature=71452b15812484b2f8a14cad11ccc73f3dbac4ff97f7505fb0d03209c7458ec2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

