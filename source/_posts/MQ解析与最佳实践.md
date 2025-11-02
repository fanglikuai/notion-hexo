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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2EGBUXN%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T160046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIBEfHgWMuS1mR2hS9k03D%2Bep5CD4dsDaqbcK67I4%2BeSSAiEAqOQAgnOk38%2BKY8rqmKrdmOcWbkjeH7T56LpRWqpYdHAq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDE974NuP1mUwqslmFircAxOaXEcPqctS4HESQ8FFNFXYEiecn4QuMHHaBih9ires1Lra7ZK6CyXrWy4t%2FW7BFJ%2BRBzt50Q4RKAzk%2FRl4WfqBLB%2BKnOHRRU9f3wvvDxK%2BDR4m4ol1L0h5MZDaTG32lwhJVDkrQUojPx%2B9Dqb0cZM39DqL7lFQ%2Breg0ownLJ13%2B37WDaVJI0%2BQ5d2XQ56Sd%2B%2FbAbR43%2F4i6knb01SSylwFOPHqxL6VPbMMmk9s%2Ft5IGMnhl6LbQ4p1GGmTyeOnSjjcEKHo2%2BNhW5nyPppC0VqGHFvKlCvZJza18ICVzWlISI4tV1yc1mYbt1YHjsGPxhWkxk3H4RpgPDZoxnusum8jUvHy11Qrq8W8GtJ9T0XAwTsQ0bng1p%2Be2%2B%2BiYUAEKSN%2B%2FEnl%2BIUcR38aZP%2FMJZBEP4f3xrA0iGjXQZQ8AeOwrFjMcqZjO%2BTcy8jyAN0xsU%2BYoDloUnBiayOj9Vk3Ge3i%2BJXHE0MyboG5rfCeVcHMOQTmyQVXU0jYYUdLEzFphqnh7Sq0f3OTvpHSsyFNkgtqm52IarpzRvIwOgtQdS0mL0ykSrEAufEBrsxOlfGZoH%2FT9YmZ%2FWz7eQDbhUV8mb%2Frro1IKR3W8%2BHSksUDQJNZ%2FNTAtIP27igIJ4GyMJjhncgGOqUBbvepcVcJmHlC6Z0IzL70tyZdJiMNNSpSirlf6fXKagxcZM%2BG5jXYzOeakVphlGjdNlFs8x8VULDC24YyLSeqzScL%2FCF%2B90oDASh6ka%2Fe%2FXUHt2tQQOlPkzDyLMExo1YvT%2B%2FY6AgW9M5mfP9VFvqFwHOzUDMvFtpOF2IJTSwoqufdT8KNnp3jWRqTGLeNXKNS2mdI7ZD5indUQLV1yNoSkWqnEKI4&X-Amz-Signature=76567ed3253e7af15e1adf39e6d7988d90f8e31738e773191d46b51bc655e2c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

