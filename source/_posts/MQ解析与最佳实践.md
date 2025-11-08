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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAZSAI7W%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T220047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJIMEYCIQCzpXhKLqDIVtgOCiB43tZ5X6RgNVUFURth%2Bz3ZGQQ5ZwIhAO%2FsAxzyac6Qov2IdVj0HEVAwzmOHyiGFxKd%2B0XWA%2FpbKogECN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzRXZWGbMSF4yOg2Vwq3AMNpfOagOSQ6dmWfn1oEuhDNvZm9w8mjovdBz96fY55gd7GH64oXyMJuLsznjq0X6W5AXlPRQSUrSPbja3zU8pnP%2F0T%2B7ewZSZMU1mE%2BGqoioc1Yryo1cz3dyl9rPZCDpirkrZg9ZcCdRMx2zLU4OLOORY8NBvDNpo887SQ5KLkpWKMf27U%2FDWlZvJkCFlD8GIpg6UTERLzjx04wcCtj7kbXfwIs3am0Z48M5nyJ3ZkY9uWYzNKSGRlH3McH9k7ROUyckHudThyVKLgZiyu8vr1Z0RqtLkPLVyroglAdL9k%2FPWOFVV1%2BjEAw0xDTFb3fRl7QAwUgIvEl%2Brth4YqyfCSCgZfgyxwWmF4S9Itdj9cYb4GbC8f31W6CRnTQuRl%2B3M6iv550D%2FTF%2FTGY8peH63cxvEPQA5FztOcO1Kyk8uZCXMb%2B7cdjkbAEyPQeuSvy0N7g5Dz9xkDm%2F0NZL6H5K0200mjqpfEgEh06A%2Bf6YwkxbjMN62AWPsaEulRBxEBAsnd807g9F%2FsKFl7lZ0ccdYJ0YJzr8Zm%2FtT1mUHRd%2BNR1v2qlGxoR5IeRYtGarVSQagzAFUw8%2FyDa7ZWOq6rkZELQIYUv2A%2B%2FVk2zzEgXuy9v8PbPLbyLEx8WJjOSTDq477IBjqkATqq2eteBGMSGenmQUj%2Fh8yblU3N5CpVlePSIkQ0pTw1OHLcW%2FpHfsTlVv1ldFie9zCSPgmovCMCYYI7O8ES%2B8ncnONlgR02fJnA20l00A%2BF7TzwmdYlyeCBFcXT0SOSLqkH7X%2FBuUaKIJqjVVLGTt9Jm8RAD39L%2BKRHO80HB9fVcpCShv%2BLWfwDFltsQaUlAKd4qsZHBjLxQk2r%2Fgynx7c8I2qF&X-Amz-Signature=0d585ab59dc3602b32daed1e34ea53a14f3cc69135674adabf273ca8fb1ab8c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

