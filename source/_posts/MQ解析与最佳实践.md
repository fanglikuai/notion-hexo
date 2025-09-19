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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2V7VOLQ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCIQCQB4GQ72634WcbM7VURxyLuDhaujhS%2FpXTEgYb6BuG0AIgYgmVvizYgM3surNmv6018dQ6x6IGWI48AD9FRRig6eMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBGwaWUwaMZANYkEdyrcAwJUnEMe485HiuIg8opDRlE2ZoNrfYz6pzU%2BsW9mp01R1qOE3xLY4WCdqugb0zig0wKQk1V5qt9Gv23qh6UwdIYcOUrDwHeKrLDIQnpRYNbabVPF4favNn92dXTy0pV5gb782ISxq6cNsC0d3Tus52qx8eXSlzivwgEWN%2BA2H2hi%2B%2Bl6AA%2B1R0Fgf6cORs3efHv34hQupe%2BR5%2B7UpPBrbwyDLD%2Bvtk12dpD48k9TAeQo8ENhsx4RWeamzfRnknV%2BoVHi65f8oivhXpgEn%2B3xt6SLd%2BJsWFRHae2goWuPZMiHvwRNvzoNuFz4IMpOC29eyTdznJO7oExiBmGxnnGzdGpJPeAtuPkFfhx7%2BqipZxKO%2BBS3m5%2FBHfNzIFa3fee%2FIq6o6Y%2BzZhYGTAOjDZzAJWJX5Wjgg9RzSsYIwLjWGd1pOMGtCOGvkLNi667px52CNQcePATWHAwd5XKvZ9RuI0qjhyx5aTGvj4Y%2F8CMX3OA9Gc2Soqfn5Pkv3ypBKt%2BJA1L4Wg0bMMaBODJflfuh%2FS9ayR9LmgZFtxd6m2LdYX5kNxwWUVNIs5D5oZ0ggRMBUvrPqAUG%2FygSszsClXvhU32Gz8kz4%2BBJh1y3bwLYdIHbWBXNIz2sD1OHqF7AMNaKtsYGOqUBoy2LVxhYGq5dWtF2klvyIFSuQ7F8b4Jgpu%2FYLfvWawuvKe0u%2Frxmit6tgq4XoGMoAPVYkG56BUQ0tAVRI%2Bu2k8vwBMMlYCnfvufniJHMvsi9rvOmjwZ7OSkHdYaP488LPBqnWaOH0OqsY5ckJMlxCDmWrLEqNSpBnyOS0cQLKqEi8AxkxRa552CdFzZly3nDV1qhhiDfw9jvx7K6xMGsMJqXUvmX&X-Amz-Signature=a57e202964fca16efee6519aad2107ed5fb4dfa8e6d63d4f0c594c0efaf7b2fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

