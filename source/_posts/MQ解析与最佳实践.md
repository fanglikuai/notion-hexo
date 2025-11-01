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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXIUWUXB%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQDKvJnH%2Bxtsv%2FVyiw4c%2FXzoLZ1PgpBRiqAvBV29oSGb7QIhAOvk9MSOvB2qTm5imSQBuQUpnGDVfJRjfYXdVeTcaWEAKv8DCCMQABoMNjM3NDIzMTgzODA1IgwYAwzNLIR53elahNQq3ANyXAgS%2Bkx%2FjfZSqoelsihgQNb05b%2BRYbwnlPzK5BUeA0foAmEOULUeLoUSHuRfcwu%2BMJl0GjyAOJm20QJ%2BdA1ytCWcCyMod1IBoi0hZDuSD1a0stFpd5HHtvTden%2BYeARyMuxl1ldEaxZGMiYkp5KItAdZ9tC%2B0aoNrWwpnZxJlz5qjVKacfZ9v1GqabItqc%2B7ce3fIqmEe7e68N7Ei7ePeLkty0LMlfiv%2Flvx9aSIbu%2FJmH5t95dcod9rvMl2BpBS5N3Se%2BgHmy4dbR22XaW9VxNorm%2BMfPKKFEBNGO55RCGE5O46dAgX%2Bj%2FhkU6iCx6SlmfCpljVl13RW3Eu%2FyeEExALikSvtMFk0sAbnnpf9rZ6nH%2F1OjZ9JS%2BgltBYjaT9TNlw2VVeKZhrV9emLFzGRwsP9xYjmERYqFDJgyLFgI1PinBjPuj4eAARfMRCFMBScs708XFIavvPr4JDJnAUbo76WH3QappRcUgK%2FNmMp5JRM%2FCqV3G5tJMra6wzI5XV2ipIT7w1QznGfBuSW1bmtrBuGXFzU6xrhuGRYUVE%2F8qy%2BCYxPPWzdRLvFIp8KVUtN0cbvhpTf31r08thmntn9%2FbxewRwFk4p%2BNcF9oJ1dM%2BYC7cjOiPGFwhhUjDixZXIBjqkAVs5o0XqLCzcJrxcPLdp%2BbCC3tl3jcSWbOUMj2xOxlOc7%2F9szEEE6K9POl%2F93MRLB%2FBMIfbLSYs2%2BC3gJSOFS%2Bs2X2bmnIepG2F6mjNJ%2BUrzdt31blHT3iavt%2BSdElIgheAhwz0sp3Yn3tTFTyM5gVmErVDrGGnUUcYzTP4GA%2Bs410ib1Qd0c8J9YTjdLeQC5f4%2BfhcgctY%2BCCdvq3bx9I3cGpAc&X-Amz-Signature=735fb6c29b24b8c2573219f6a45ff8f7489d04eaea048bbc8f5be4b035e59bc1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

