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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQN25MRN%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T140124Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICCGucY0xLcT6ee5GMAP30WT%2BNdr5hF5t8uQeaJe%2BRUvAiB6%2FCod79gXbVDfWHN6Er18ceryoXX%2F%2FaaR9g8xX9Fbrir%2FAwheEAAaDDYzNzQyMzE4MzgwNSIMvhHQjuE%2BULhQNfA6KtwDnsOZq3ji8vRcIwYdCfRJdIt2Zy%2BTniWKcVfCDG%2BAlUl6GQCwnq2QnloMY1428TqLmXbPvDFLwCI6sr1glFCfB57qonf4zyNdCmhEdoly0kdENU7Z6jX14%2FcIvq%2BbliFJ%2FQh8bJdxI2GvobazYdr3YdzCH41dZFZAFOEF8D76Z3lwRQzw%2BYf2fKlJ6X8TYI0Kmj0GxORJGgfRfGhkpStwejsGTrIWR0ewzKIW%2F%2FCCZdfLmlzgIKjxr7sSECq1bUq1ACl71J2S1kWjoeUTxhDjqQokBSALevOB0OACHcgsxR5fFYBLn56zL4gyMWmJK%2F8rpVxMsXcuAHK%2BqjtQABJvuM%2BurCDtIyi2PpBlggjx0cpOzeE3K3CQn%2FVEC%2FNAyDVWs4Sag0IEP4KO8x30gtfpSAas%2F%2B8b8du0QAMPNOWPa9JsEIIvmnV6ROSdePjdyD%2BW9AqAPz0Yf31s9dompewLKW7J96lXAPmsZBtZOazkSF%2FJByyi2dtz7btr02%2F2hdzD65Kdv5vQHAzp7qE9LKJT%2BIhRUfhu6fAVxJKpdQHXh1ZCPX9yI08n%2FWleLwgAWtV0k4ZX1KAv%2Fqqu6XgkYqKnasqpe7w8mb6TEC%2BW8B6jXMCTHEOljI0iHtiCErwwz5a5xwY6pgH%2FqnJjUgS4QsQCMoK%2FrH4QZKMtropLJXTHDIBmU4fNdVRvb2xdfZmurZAkFQGDFrNL1n8UEx3npErc%2F0%2F2xogrZ0VQ%2BbdekMdeFRtxA1YxZVPnfa6ImZ1LhoFGnPSOJNmLpbpHgRp%2FVhdOFi9V6hBln9Yb8L0ZXiXwQUOWUhymKmsXJjzwhsjor6MnQpX6aY0PuuteJ2QTO4K5OJiBj%2B%2B01IazsMqA&X-Amz-Signature=37a32ea3d935293c8480f35e09e31c20793d3ac3bb243a88e23ea6e74ceaee3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

