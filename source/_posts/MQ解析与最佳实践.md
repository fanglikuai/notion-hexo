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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXGOUC4Y%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGeCU0n3Y6533HsA7T4JOwZjKbi%2F1xFbOr%2FUakpeRs6YAiB%2BbYXY3ydOlmxTy9vXf1HUuS7UMZuEJt0w%2F%2B8hfgRfsiqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAyWJ8PynVPY2z2CEKtwDJVjlDnlGgbgDwxGPOmb56kvKGM7YHK%2BoDQ5CbTAeL0xqxrZj7QM%2FQ00pO8T%2FzzBLQGcdwyL4Uj2eTxH6Ilz8VN%2FudKl%2FXdXw1BD6FK6F%2Fu0%2BLjEsRXWqz5eaP0fyxxnBihgJqCPtIc6C4OlTh4DvcrPEE5y8mVqG8enTau6D7KgJAeKIVgygfUB35%2BkItcY2NW%2Btwia8hFlW1jAifmDXwK9kJjkLo%2B940VajujHx8tdGoViQlbQYniLmviutqx3QGfofN6bC%2BzK4Qvc1NNilfhXu1j4f8TBMuDoMexvPv7URe0lpSRazS2U8y%2BfObAbyyGv0ghn3lSGFk%2FlXkWReLdr7jUEaBbexAlC5Ho9pQQ7CNkNWcqUVOgZXbip5WDJq70uOUHXVOZPhtR94mOOykO2JwkcaVNPpFB%2FioFtJeHhHchDalKnvpNczwXBml4x9IRbf99TEAQ5Pys5Ojr7WedcdrKPMSBCWPvdlrvfvvbg8MxaPdsonoQjm0U54k71A9A%2Fqdk9CeUk1mLJ5CoToVbHYAcxzveRQFCHx4dlgt12Ertkyr1nZLTskZYpTRFIqOB8hFpG9OYPK%2BydvDePquObOlXbD9ba0DPEVa0sMTGd%2BxPDOaXPvFlFUPIswxNT8xwY6pgHcTIl2OMb4k1qeqF2XOorX4d2qJEqP16XwOP4GPlWxH7eQFiVIO95BgNLQly8ix0crddFxAXF1OTH5VB4YkysNFVuc8oparSFdRspB2OAZzXV8qz81tAMVrEGlayJ7O4%2Bn7LKqS4RRBQHM3TJzy9NRuR335ZfryTqeWS9%2FosVMvImQkqFD7pZFyzcRO2q721zhL7%2BFGIEvp8o%2Bk2KJFr7PRxUzMAmO&X-Amz-Signature=7ce27548be041a9c3123c9d2fe32879a055d1072988069bda85a548bdabc2ab2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

