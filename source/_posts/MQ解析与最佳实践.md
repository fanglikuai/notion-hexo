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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6ZICLNY%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvFlrPUuv7xVGaFpCVzqNOia%2Ft1zrWdEPcso%2FgdqO6xAIgSLmetP8E5c7KrxP9%2Ftxaq6TX9RZ2pNA0En%2FCbTAa6TYq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDGqbwd3OdvtlplPxYSrcA8vPSajzN6Z5sdE6fdPpt0JhJ6XBlcpcylXy8FnvNaxJxCzOwuFpNLCdjrCx7D8g1AZapjyGYOitf09ZVUGI4dRcVTZ37cxghUZzLioRUaQTHqV7DBhmqS03ihGDR4KKdo4DJIEsu6BNiXBULcERrDM9yoCOaWdFoFhvymiWRVVYhzBWftcWQJ7X1pCrvYB7F6OBLYpGZ9EuBjcfxPFekD5vCXcF5SifRG31Aoh3rwz%2FeMAR2HT1rI%2FXNyzSBnXh1sOn0iSBcMvNH%2BciY7zczoaTxfZq8UXIr7seoy%2B5PlrQ2VgcyDV4XlaYVtpnOw9q890OzeQvoVUCUNKlfkMYMJ2A1nmkCQEo9xNQr4%2BfUiS11sUjuLaSCUI0rMKiuNNqbg2Di5XsRG%2Bf7nUuazt48HSyIzXHIDCJoysJCFy1tut4sxBTFo8wWvcs%2BDJtiiVsL96ExL%2FontwuK2eGNI873pBwkZF9Iw0thtUFfs%2Fo69q0iZvrVRmEd6jbXGSV%2FkQ7cPIkRz2HHNwAjzgdrWpUTu06zAiJXyT4CKttjrdQ2h%2Fp1%2FcVMSWLgru6UyJ4vpm9PQMLmBOeABIs7X9ZlTqceDSGaWq6ZV7slb6BjPrz7c9eQUrFWzXrIw1epLJAMMGAqcgGOqUBWSl0o9l9NSnOVAZTK2b9eI0QAndKrBKmr6YJwEDzmZMJYoxTYx2Nme%2BboMr2rcHnTFrm7ggR3k%2Fh4BvnyfZb755X5OGLcK46BKpA9D8acNtK1kl0psoSxrNR8tRRgkf8HzGD5%2B8lQ0IzXfUYAtf4KZJRD7bxE3MzN6vMmwuMsZuxnFtKXAhxUEajqkB7LVdmBBlaa1RXtLgTbHJfkd1DOs9bcLbU&X-Amz-Signature=caa796a898dc0d68ce6f542a9b59b5504f19dfe4d9a8c129ef8a6e5664c8f46c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

