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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIMESF4R%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCpSzF8NUsX3ZfEtZI57I1afX1MxAQY4Fqa16UIpzTZlwIgeRmTg%2B2RLrREcgGmexLGeOKMijiALX6U7%2BHjjqTHqJcqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGDUPyxs6fiXNt4AByrcAyimqL7MbTVmFbLA9DKf%2B%2FOAvhUTSG%2FBmPwUnNOx%2BERN1qrM7XDSUvFHciBaI5gDpENz4gzZAxukGHdrDDQxhH%2Bwg18KawGM8z5Dj%2BoGMxrRNsg5KxQsdeNpJWEyNov5RECVbR8ISmVXfh%2Fte60nQIXgCtajWm2ibuTX6Un8UbiGWorPLxnB1BFAGsFkraqvkgoyUa7NkPvvyvMbZAhTvQ8MwWixn37%2F7X4et46Uesu0SGDabf98HOCo8fqVIhCkQkADu%2FHcCH3muQC6fcKms4V8%2B03DaackUVY04nZYYc2wFyDR2%2BE89dRrpEB3ETPvXkvLp3HTQAxuFr6UpTo4wZkVKBN5X1S8485q%2Bh77nSPWo8AQefOzplpiPBTEZMgUSOXdijAhHbSbJUcge4%2BkkvtrTfPoYZVYlXKATMIVgiwbq0QdK9QdYlNS2rszQf9LQlW4z0YZ%2Bm5Pr9JwKNsxwx%2FBMi1aJ%2Bbt56QdMrrBV2VAwCYxeaEnK2heT9ctznp6MhHogTwrYvuwpKh2djjoMzIOSMp8NwydFq%2FoXu1ccLg7Adgg1YnYegPxfANQ%2BeZBCV58G%2Bjl3TXZ2cgyjfP1CmLU10JKaCLBLI%2Byatkxs30RxDpR8H1DGmiKA76XMI%2Ba4sYGOqUBcp5rilZCI1%2FfqJRFX1hcBLBFGfKCpKLY3ZNtiy2roU94T3iUhjDhCZCWXQr9crdmdPdnDy1t1Hmw97zXISVJfj8RopPq6Rqt2sFRSl26FtIsxgioF3t9BJyF7F5cvIiQ73a5gIAmiobOJkbgXiSOgbVlN4OsWf2FtDtsnwY2wsHhqmtfxTh9X4bGMEDUeMapbCr2XpZ8NKS8sl3iu850PJ3HJSog&X-Amz-Signature=9e1f96d70956bd9575a8ed4490ca9ea006c20ce30d46da8dda0a18437d1fe89f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

