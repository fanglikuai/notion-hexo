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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VL7DEJDO%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T070051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGLwAu%2BrfTEjqEU%2BSJw57hSzX4CSspAXKf9XZb8%2FSY32AiAx990Nm45SU0%2Bzp3uolSbSq%2FhaGfhs65vhEpCmSAgTVCqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaEiCTHUdAnHGT9BnKtwDBYjzC3%2FJLqa4Cxl23ojHzz7U2T3hppVy4NwIcfjtWd4YDi1bqRKo6et6lOddfwksH1UHA9BhILGg3KwJUtzEC%2FQZOPQg9a1J6V5ombACBAsIUl7we13LxbIIGaRrH6uBG3xEqQjUeujA32qBu0rhhPgf6F%2FpdtHKM800%2FRrv6xFrg0E45eC%2BTRiHYF28chnpnibxOz1d6ijycWYdds4plBhAyglm9K8o9ovorItF2jVDcskMfl%2F0eftiYAchOeymLwEI1WkC1CBQmvXkNC29WduKbsQ9egzx9WFzjmnsXYCyevGpgNp3OtXLcGqCS1vcoqdUHLj1vyVUXc6Z%2FsK4wURU5twK%2Fyc0eVpz4GuUHCsd%2FLsPINPVWbZ6pXSfHADR1wNZZmpKD7IOjaou08G8KCMNvufz0yiehon0KL8oSvdvGMuJ2EbiuxlzuwQRGImypMqoowx9WXU1lZteH5jqMI6lY4clOmZy3QLPCTbOxSCK66G%2FEzoWJB34viqhG%2F%2FvQM%2B71WvZRB4eo8J%2FatqyrjSDjRJe97osAlXS1UDHq4M86KKicX07fIoz1Sz3YWVR1fZaAZ99gnbNgLRXqHuEBvSgJvV6Scxlc7y%2FoC4j7yjPu6cIvcllP0h4Q8Uwl7%2BNxwY6pgHuc3uID2YOgyzlfCcASi1APXoSPYMCULH6blpCGShc%2BaV4mOrZpSO0O2vg51%2B9MSJCtM0lczp6l0BXPrDzBFP6iYiUDWBKh4emcKaQZNvoQ%2BRBSZdOr9Sg4SeFVo14Abkld9EbxMCAvA3eaxMnitz9XJMOtDUGQEAEWBv%2F%2FLx551G5wCF2%2BHHswFPLnQMgnm8WkVwQ6D0qPM1D7%2FrK02BPL4oGQ3gU&X-Amz-Signature=8a93c23b0160fa1ee8153c4a81e5e9f023b9ecf85142399c9840953f5d77fab0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

