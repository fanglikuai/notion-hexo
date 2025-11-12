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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3SANYLK%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJGMEQCIGegAY%2FJ4ODMC490UlFnzey1Ex3qzBTGn5P44Gb3uCqqAiB17W1wDBgkCqxoTdWkQPNX1jAyCThm2w5w1BAGaxsJCSr%2FAwg0EAAaDDYzNzQyMzE4MzgwNSIMiYuVjbTgv04kBTXJKtwD3GYvV5GsuDbUg6shKA6aEAWOgzJqAMtQXlc5U7bhM0Az530EtCGXQ5ZGSL5Y0MRJW9OTuaV%2BoacKhNDnmcAUnYhCqTBXZgmFrSskKsCfDNAZM9XIJS8VXt6fijaTPr1pMWPrfcNYFQBAg7V%2BTqTk%2F7s%2Bch1QBH0%2BI8ygt9RxFG0aXUUKYMLfFk3BpI%2Fg7FnPm2liS3r0pS1zSE4v7Y6k2IStgvsSlHjx5lOIL5s5i8u8KXBMfr0%2Be%2FhT4h5izmQYwFGB9ObVEOjmSL7H8gdxMSAjJsVFT%2FuzeHgL3JsHALTdGU3xJAzgwnz9P0IGM6QXcNO0wk7zqh%2FxvgSEjq1VCxu4ROJjLxQ68qtjm1TAEufJYfIByuvJPLjbDMA4DsAVo0FzPWyTtJ%2BbrKjE%2FsZP8jEvYlDhaJnnDIQQOECqxCywQOZzZc5vnScoeDlamc80gYwCuRMbwCZH4VlQRs3WTN3JO26Dr5rQLlm7nVjUc5%2B%2BwEq%2BJzFO2jM7ZRvBS5MEtyhC9jfUBwmm8sPc1b2%2FKmSIHCEQOZS90fJ3tP5Wfo246rjeP8XD64XiSqrqManeC7c58zXtMEa2CjoHAqzgrNgcU4gBRMtdHrok%2BdxjrrfClQSLheOnJdMfkTMwudDRyAY6pgGz1gWpKHsq39ysTCC2yE41iSejrNrJt4MHOh0DrCM69WQqMU6wQbHKXlQT3%2FglzFmljvUUBp21vadHv2Ko5rVdlEwK9uTEHl5P15phsRPWDDAJrL53kH1H9TOsix2bWNKUOS1fULoEzjWYyV4zCL%2F8oDAUhDcgs%2F1I6jP66Ot7M5GJeMULrRXimetZ7hOLTqjZbN%2F9oehcxtNLmmV%2BYYfKXig4l05F&X-Amz-Signature=0400d41b98751e5aab2d29eae63fa9005bcbf9fb1e089acb76de312d5418a7a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

