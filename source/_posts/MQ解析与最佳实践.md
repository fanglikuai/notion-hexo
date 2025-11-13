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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKBKQJD5%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJHMEUCIQDotwumjP6BDJzxMN6aDH5JLgr9iHZZ68Vc2sT727PlUAIgIEQiV7u%2B8Y363Zl3ZpWtRpeRN4faACylzLhvB1yzqFgq%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDLb8sGbCVOJdXTUzwircA2iCiqCNgXhmBDwnDrJdr%2Fnz5e0VFbHDqB9%2FB06E2vjwH14hiu3GHQcgVPz7w0UG%2BWG8siwkqMJj6nVy9iONdJNW4J4oRw4%2BNi7R%2BrIto5jv%2FPlUXCzynKI%2BxwdvNbyafwSjWrQ3EeoVst0d%2FGkBiDuHLxJZNzzF4WFm94fg5biNX8oMyUq%2BdZEqS0Z8UFPCHLIcOfQRHj%2Bq1JXT0%2BF9XeiHtf63RHwdWl8c8FpWL8vi5p4Kr25rqkd85JSn3TrbrfkQzLv4YxXsJstBZY7xjBJDC4bGzxGmcGs5vG%2FPiCd1AM1elSzFrdWwbADmvIFWjSNcotmnxxsjvHmnGLYEzTA021kTIawdQCwZyQ8W5Dc7Zv9TyCUZKZIo9cUUtOrKB567E4OjG5n8Fig6YoWhIV06FxkZitKaFb2D95s8pcG2P7%2F3klwt71czvJKTAu6xSmn771yLByoJwwUUxB%2FWphv02ElYlJdiRyz1m3Wjz6WB6ZpBNObBg%2BvoToDRYxlDaVO5qQZX%2FfY1Mz0T7DaWl25KA5xBBumy99uZN6bLV%2FNcIvrgbxGgEA48pBQx1Z3igY0Tn6cusb2eE6u0b9%2Bw6DxJ4FglynaJi%2BOHJmhSnJCSawiw%2FvJVKMWM2xLEMK3m1cgGOqUByGiwLxtkgwvdLENT1dFQOAxo%2FFX4YoQnYRW95sw6yFZPgJJoJP5Z5NHRs0eGyQgfPgWSlGuc1eWHOxBcrAiEfniVAGz7twdA%2BO8marsH%2Fwnjncs69kLCdLnLKz8qOfSqEcRLKywA9CwGrS87vFoQB7G9lGxXXyMSuZP8MCGIyzW3s6LrStKq8DhF8xbyP%2BVzo%2FguzLUznR9lSkz0P8fRK5PI0ORG&X-Amz-Signature=7dd2cdfbede1780a7d0203c86e46274c4627f6703fb61328e00685b7b2c3b43c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

