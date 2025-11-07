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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEDQNQVQ%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBsuK%2BfsFPEdMqvfab16OQFGAHd1Dw0qRPKJHzw8ZHkBAiEA6aR8UwtIQNuGh02v%2FoDwVct%2B8GNsNYEAkyz3gHRYElcqiAQIx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBgIg0%2FyaLX43n2vKyrcA2Qthmx8dBarDgV%2FLAEkgoCaAYRadqsYGvjyaSUF%2FtBG8Dcc58lx4rR8cef91OU5WzDeASIgSz0EdzDweXrF2NTmq9mgDV%2F7CRi%2FaoHLkm42iJL9QsB6Bebo%2FhQa0h6omkWuADLLI8bOYOz%2FE5j6dbcJ736R3H1cuf7wIudueAMsoabN8cUJwSj%2F5w9%2BtojqJ9qVd3oRm1mCwxKOxG4BH9CE3Snh46iTEfYlF1xC5ai77Q8rRGzipQN1eDlmyh0H31x8i6GwzPZ6mLOML1M9UMeBO3gjArIM2nZf4HGBCeNs0dN8PrFZ%2FKAEQtHNa6o%2F92SiW1X4%2Bq998FZ2jDWA8Ziemw%2BUN7RTLNtxfNkW7RsWhEAZB1I9ibd8xtW4VHDZHPf%2FmSW%2Bqw1VGtVUpvzjQeYDlKh6TnNR2SA1piO%2B4VZxhKZ%2F7%2F%2FTW7FqMBBZFIdrxIGgPI7s0izhMWtVph06osp3gz4jjNSufp4GPG6sZV%2Fasa6O4bezGyoKmfkB8F286%2BdCJFqiJbTUMdCwzF5LUgL8NtVmHcUpLvRd%2FIDPzfbAKmEOCQPSQTIxhVTJ4aN8Q1coZ9eLKHPNmIFfAOzEC3P96TAvhEA0PdYs42M3es98kqjr%2B8P7lKylxMUtMPHducgGOqUBqhQwCzwWEVA3OzVP69w%2BziTt66QOChbkVvgo290cFJQcBWlSbCZAj%2FXEgqYO9NcV1okSriK%2F6LkWrtB%2BsjUeP0zq%2BiVdwr0UyucAQPMBs%2FA6zm9hvpRYcMOp%2B85pcMZ0wteD5lDQUEG50p2EmKl5idpOUyoI62QxBY%2FU6vFoKyHJfKQfcN%2BR918Gq%2BkOhKIP1xkGuVFMd%2FFBs2IKN8XhN00iNNAR&X-Amz-Signature=14f6aa0426ad10731479c6c61745302ee481089d01b1e8314ee56b4ae52d5eb2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

