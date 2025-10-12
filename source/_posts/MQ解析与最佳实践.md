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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMNCCAZD%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T140059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDwlafPsOrx0BcTLmX8J7c394PApPY%2F9rFOVBsNTPii%2FwIgBwJ9iXKP0aNmnTcB9KGSJgxvhWtcbrhBbRAh4MmXurcq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDIKZrXfQfRo6rcK55ircA7qudbeXcGXcKIRKxbj6A77%2BZ9150B9EZWlzTsU3EvE9L5KWcfjWGXAVcMHfgDd%2B8%2FLp6piJ7xHx270K2R3WvJs95SZzTaaao2weFQ%2BPKQ8%2F%2FW1hFjjFom4fwhY90sI12XNDvoSRL1om0eAE2VhnzAMohNGLXNC1b27d%2F9qKj%2BGDssxXhUnggk1Jklp2jsalATeObOrmE42ozPZ%2B3RZijyxQiVGJ%2FFbbyVR7AaLqGW98n6sXuHKIH2KphP3zBiM%2Bvvoa3v1zESJyorDFm67dlL4%2BqoC1j%2BQRREUCwMCM5UVbMDebSV9zadL4U%2Bc5joLx6%2BgS7%2F3qH%2FF9UQs08qcmoy1UDp0zq1%2FEbUDJxRFtTyTW33P0MdLhQjQfSm9Yvsuh6JzNe7%2BDB6S4R56XIgmqvrR%2BtJ5yr%2F%2FleRSjiNBucMhrz4hf%2FGNKdRdipA85zPeYaQm4Nop1p0UvwmTd66OjqsU%2FGimH5%2BCR4TJ6Wdv3XFIEPzZlN1pA2L8QtfUXvDiBylnDcVI8LX7S3UkMZLxrI9kNKWb5HW%2F8mP%2FMqcJGU47qmr08Cg%2FUluTLLeYUXSBU0OncNllCBYz3ueJNBubuE9SaLDG7kFDXNpQ5CE5gVA075jLgS37RLRx0JrJkMNq4rscGOqUBKDNzpOSukaD2hew8VKFyyOgaTGeMX5VE%2FGpNadtmnLMKRuctXouocOKU3k5xXsLA%2Bi5dxcKUudosqpydTw0dc4iE7ivH6wD6TauSRljnHzN9jroz1qy8Hx7R7a1SoVB8cYyYpX4regZ94jyV6rt0NtMyq%2B2NvHbXsUcbW0ulu%2FQd%2FWg9GZnsDF%2FHdukPTmUpbGJ6DdaBn5Jit7EbUlOI8CRojTCb&X-Amz-Signature=c8e8e92315338bc33f7cc460a773f8c2d0f9772b35dd6d08e0dfa5a91279d91b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

