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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXQLL6Y7%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T120056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQDb2a5pq%2BIfxn7SL73JOG8H7xrmtEMpNpbVSQf2thIxqgIhAIHKPZgNJPeOguC4ZpUgmOdsleGu9vz%2BK9mRNnaSVt7wKv8DCCQQABoMNjM3NDIzMTgzODA1IgyDUourK5ItO74aAxkq3ANbcmlLZ3U3lUom5n3%2Fi7b7z4PQKESc%2BDOWI9nu4SNrzDoOzYJHbxu2h1ydcoOIrfLPWbQtUSiUUET41Mt5BkzDnVTCqXhimanOoMcUFfDXWN2j4swBPY6%2FNoAVYdGmkGXORjCdpx%2BBxcyKeHIMdWVVfRxYOIZpLQqrv6XdIZ4alZn5mwci0DgolSIm5pyxzZ2O%2FZ4cjLjmZ31DNsrtrIvnwuTtIrmhIfXXNwz%2BP3wtnxJtj9GN6LQazukO6EYLGM7S9CWkE6RZ4SHEyJPSgSJRTcZt71eZqpGW%2FBGd1TsHTNrcVju1VmKmds9xcfM0GJosWeO%2FefT3iKKHckL99v%2BvP0bIOEk71Q8QUzsYHX806yf2O9s6vva2EYpcOmYNJZxukRNG3Pk5dKNdEme23qRwu%2FCWBpqgF%2FNDuYH0ZNdUVRp5YjeeRBlokcPHU%2BTWZ6SfCWvWHJSKquGwj4Jh%2FboXgIml5MYn3c4cNQwHwIWEH7VKZP6ic81oHEWvi5%2BHVBcCM%2FVpw0oRb6sny2BZUo1eNaUlXQjmqrHs0KlPvAesjydsW%2BDQqCwDvZ3tXLEtBuke9Ykr1o3jfM2kV6CrGyjBo4SlS8pDbOlUe2dWVwh4rLKhrRd1PhNrVEiTbjCKoobJBjqkAcBQTlaeX3U4hl6ieql11svCUF%2FAbZj4f7XOhyKvb0QsUrbKSJpF39g6rhWGdbV1uiNhNIWtQdXqzMf6zSjbSKl0Vv%2F%2B%2Fl9sigp8aVsod9HSm0kPGG%2FOM9qjwuEkxP3lifg%2BDJ8%2BxE42shI0uuCRRT2GosgBnbBnE%2FqA4X09SdJGXUmp4cjeC2TXM0erhV3naBd%2FpVWHpcrvypKO7BzenowV3Qx0&X-Amz-Signature=29f5ed4219051139c5cb71ef01284092718a7add580c358f1a4ec9e56a8c40f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

