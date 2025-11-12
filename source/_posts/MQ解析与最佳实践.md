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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSHJ64KK%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T120058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJHMEUCIQD%2F6RRxC4Sv2sMFkkcWhFw0527APCJsb6eDIqyeIu2YFQIgLEUBeGHeqVqTO5BbABiFtfJjRXnnJ%2F0%2BwPd8yvIww2Qq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDL%2B7rNz8wD83e%2FHyWSrcA3drm3IrYGW%2BrkEcHh1IBLjn3q%2FU1CCOVSEVaiQAiFusIqiNqJknDRem%2FH4XRPcz41JuI%2BqHsJwOa6HHT%2BG7XtZYau2udOlRec4PTYY%2BQndXwci6Hj7sl4HzC8ETpy1b3RArP5TK0cHHrkTHST5%2F9PVXVwSSWgl5crfCOsuSGmW1qf0ktmt052htMCJE1wDkBPVsQWIH0U4hJ9BFXRcPBnS2V5BieB6USPP1jM0vxsL6KAxp%2B1Mfdqw1GRkt8Rdt4yxzI11n7KKgYC7pTm9izo3JRYtroRkUbzbnbX5buD6Yu42VtKWVeYn1o7FlVht2SwRVmOlPsZoNmNwBJNBHeE%2F%2BPRR1MFRXmGCQTc6TwdwDORa0w181YMQkklxizgLej%2BuTaIhp9aq5yQqudiJ69GFfhF8fP9%2FG9f2ZBTAI%2BEmXLDqcgan8ZRA2%2BtWeAP7qE6DNwPwZ1bHUywlI0vI3hovk5Di639FGTGFtXG9Bgu29xHSJAan%2BTZGSjvfbqSbkY220T%2FNkoaR0%2BiwsZzsQwAfJlOG0CrYi5AAuld0fdU0Um74Pti0igBO73vzz8MJbfoIT8u6IOSyDxRxpXFSNlhvjotSOG0WwZFgJEFzL19Uhwyli1C1FViRkDBtrMIrQ0cgGOqUBZDW0OkQMIImILVwroL3uSw2xSqxuR%2FDyxj5SdsIm%2BUPtbVlUGT4B7T4kxPKIQx8vaZ7JLPKJyYAFSk5twYig2fwBgfmdDeNf9O%2ByeWv0mBm5rDiBiPd9omrnhSgkiWsF8cP1hjC6zMuLQPAMgCIi5ged3mIei3OVq5o6XM%2BVYzjia7a8syQ02GGJZHkRiOEbAP7s11BeecacUX3FgKbsND17H0J7&X-Amz-Signature=0bee5e331617d305c7c457e8c4cd32725da1590b0639b90baf4cb19c4aacc8a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

