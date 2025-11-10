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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXCTNPSM%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJGMEQCIAe7rmbTa8R1dVqm7%2FUECZLDTna0z65fJNfih83gPI5KAiAazKKqgwQ59FCsN6NxNrCL4YEBvKrIyGx9%2FcezvjVpByr%2FAwgPEAAaDDYzNzQyMzE4MzgwNSIMb150dH7BArtxrGMJKtwDRnJDAd6fkeLNKIkMmdS%2B%2BkuwrK5%2BD%2BpciVQydHHtivkrXT3%2BN7xRgHFlw%2FLFIpZDKVb%2BZVOjf%2BjenGBb1GZqYdDINdqfS6OKwWyge1P%2FL7A8Ntf05nAsRl%2FbeD1sh3YfI34VEFbN0RX7iUY%2Fei5HTfIanCAM1K8VbeXIcgPCVxmh%2BqLb%2F3mjTdgSetkt9dLd5MplYLWkOeyPFsR1NOuOk6T6hmN62yZpO%2FZiS4v8dYWNxLSCljtV0Ixa3plfWJKxsl6DKOYoKEocozcqGIP0ocZc5e7%2BbZUpJOQXvZXP0vz2qVbP5d3MQlrX8rYRz1vZ5bcVbxREZY9tqbd%2FyrHpDX6OiG5ZFOMUyrn1ZkngvhCMFWxL08XCAxptv0XZzRChmuzY%2BMnXRoUpGkenzAp0%2Bd0egNTxtPuLpYaWsYWwTvy3NHG5CEzP80TuYn1BdLJay8MtrL%2FirAP0c8VAHdR918sKaycl1c%2B6KKhUDbx%2BD2FGlZLQny4bbYyon0mXUgArvuVft2sWKxvl2vuRqLCwvS7DFBwwd9zOg2596VGBPwPoHtZy%2FQJWZ3n2ODHifBh3WC%2BJMj1DLNIStgoBPQupYgqeypyKZQ%2FOwtwUdYB56acLxA1lj4orTld9f9IwscfJyAY6pgEcPXYJk9OIxuQAgp0zNZVOeogyECyuA8grntTV4AfGnFH5LCPizbycRuE2Bt0bkYB9jrWkUa5egbHJ5pAuyqKBM7Sy1eL8r4b3SCW5IuO649Vz7VDJPG0Ccx0CvpXvcTXL78rzWxoCpHTjN8VVml82G7uixWQLPf7G4SGMwiiSYiLwyMqzB1isEk2bAs72nZPC6VcBAg4vt%2BFyny4g2YstJ2V0NlXa&X-Amz-Signature=84ba3135910180e6553577b91cf2eb36ac6534963ac143d41274858b9777ea8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

