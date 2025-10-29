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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4XIZ35G%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCIHajrup75yOxHNoFFBK7Tzutvys%2B9QVGkGQIho9ABUHEAiAz4hMv4g7nnbq5oRkDJFLY6KMWo1ZGR8946Ns5KCxW5yqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTktgDAEedjAVmirJKtwDpePfm4A313CFh6S582rOoZAWre2z7gYZWV%2BQWnZ1Syd8oN%2BC%2FX822TwDOTXMXW6RKJJj45YXbHiuh0O%2F%2FYUT1tp3vbkD669hnkMRR6Xx9WsDgppTXplTAPUpE7PQUiSGGbvVg9nnCmDaHQ5%2BWdebBljtydyaAz%2Bx3xltj1diWVIRcpVgjH05Wo2yvxjlczUvBI2C4D%2FafFYungKyU9qkBmyYiWyGZTA8mE6VmqjiXnJiWS2Y5aRAXM12r%2FKkaoioQGcezBKHTlrIXDznVHSV4%2FqZ3OCbUx%2BKDZbZJiUYKZiAknlKPyS9QZ8Hq1xjYfW1rO36%2BVf3m8948FfnPKCIs6%2Bg4vpdNpkZsOgXfZUYrFFti4iMjv92qgRahcv%2F3kKENyAOsSPgQA57dUeVvPi%2F%2FbsKPnW6rJi8L5STZ5qvnjE7hgUyk%2FxBeAEfEz3TrTQE8nRuD8DqP%2B1WDYfetT7W5MFipstAMgWPJhqsItWruNGXW43JWNZnXd2zQN%2Fc6PybDfsTVbbpvgNHoD3LrFxY%2BSRxK4mW62IJcdHB94%2FiaSZ3bs6UPuVhQZ1qdhgm3Rdgp6N7Ubl%2BCbDFTHP%2BDz40qIWhIo2ewSJQqHerh6NFAo4%2Fu5cw0T3A1pmMF1MwzZyJyAY6pgF3K5UftpWKGuJdYu5CHFBC6bOTK0zWegBmrj3Jlfso%2FrHBO6XWzNFV%2FlQf2wmMc8zt4Ffewe9L7lUQkPDsrkxMZs0Q%2Bwo3WF5C6UdXanRxTkAkGoqE96xRPgBrDNR7FMQDjmpdOszVjc86lqzzGbhgI4RaJik7cNFTRQA4wBmqSMFw0tmKeer6ERE25CZjuo1VBkfwn5pJm8NXpoort1Q46cFynzc%2B&X-Amz-Signature=faa318f5d100e49c94de6f4f9c73a6fac1dad4466aacfd0bcda9e97f374f25dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

