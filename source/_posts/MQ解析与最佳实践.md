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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TPMUGB5Q%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICtdJfzrT2y061n2jJ%2F1Gop%2FPzNdejZxn0BiezFipig5AiBSWdgqFjJmvV5RSxxyejZxVhvcLai1Y1yWsYU%2F2cbpEyqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhNd6B8uVcgECethLKtwDxvLjbRNR6eYgrFl6ktiMqZpR3GzY1KrgWnblEBcqewrC4ujWeNadx53ppwoOgqa0CNWPaxd6PFQUYP0C4QjJaLNzxrrqHUMarF0eeQrstDMqtZWDKUMJrSRxHLlcUgTCfIpLkqjQ9USHN9IgqioRkr5OTbnBO3io8JX4Pt8WUOnuVbzgeWSfSqnZS4rW18mu%2FgddLM4IH3rLJ6WpqeyYNTq0qnforfojHDtcQ%2FDlhzoQ%2FOYo77UQrbEyvQ6Uszkq%2BJMKYhL66qWHdO7mr%2F24Pn9%2Bbr7d6nQvQCeWQLKguqq9MT48AMm4DWUfjVj9TWvf0ohEbD57OH38qqrAx3KGhOjQ15JZR8kl0JfM%2Fp4gN5zPqbPqhDXr6yx1UiVfnwXaiOtmf%2FYR6XgMNdFhvpBXj2PsSSzAx4WoiLGiK24pXZdIlzl3%2Fi1zeInXsOXBjV1Q59oWY2N8o%2FhKqYAdHC8p%2BWy9E3WYDHMOWPtZsSDe4gMxgELs5Wv7Oha2Xx9YXmwkA7dBoLn0wRD7vf1Byx8KoeYDaykrnHsbDGY%2FTI79X2uedOItryyUNTYZ7h7FrhVzPFP3uIvlIJEHw1tVhgrl8UXmbISMxtN7EFES3AixK7i3h2Qm4rnq%2FfrFdjEw%2BqOyyAY6pgEdHKPk80cq%2BdGkTXJcW5u46SA0gRv4lAJcY8VLD8ECh7ylWfZgw1Ip22Zrh3jkMciOMBY44CwTn4kbkbKjZTICr6tZW04j%2FKtP2Y1VLz6nnp4TJOJ2FAe4EB89yAuN4vkPBpqcTX77jKEYT5sAj9Ia8SLnviw%2BmnTkOdLBo%2BcBCSclbTFT6YaY1fpjd3g2SQQdcIIPx8Zt3%2FDYRSkcrJdKL6VWro6q&X-Amz-Signature=e40fc83fc7cacaf7089be846294f280adaaa94b8b40706a6ec6188144c66f363&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

