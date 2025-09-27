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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664BG5F2A2%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCICfv4O4irpvDWi57qgsQArffDAgeD2w8SFYDSVuu%2BsN4AiAQjWGCw7ww1SjeuHnDobL7gR0%2B4m4ShzBCf3p67SoHryqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTDE813ZHW%2B8hrxuaKtwD%2Bhj28kieJSP2jN%2BcZqmoahdy4zEsK13npUW3pHKK8MHByHqkk96156h5brX6aIN3rRDsqY7CletG4MnnsuR%2BATkLPV3r90y8Jghzh3I1it2nxdXr1jpUujpOsjg4HtRv2E2EPn9KVPtmV1zLNNlM1I03hq2Gf1bLHNUoief6QCrszlv1dTUeE7zv8HnPC8Gqonrhig%2BqpJq4oXX2z5qL9dNIOj98YnrU07bFeTpscU%2BlFN%2FREKSZ8MrnXAN2E1RzO%2BhqDOV0c7uIa%2BpFHMTkx0uZUl%2Bt5pbq3iwF9E%2Bp0Hr54yryO4Go2gZY%2FQpjWg%2FWA0%2FZYWstyVE6lcotIQS%2FWjBJtKC%2FkXftHnX5QbYcK0Vqf7T1FSZIvJgRQkRNoq7NjP7bP4wKZTuFZ9gHMN%2FY8kqH5vUJosSzUf8MLGUMfTnX84wIkSo8y%2FBnRqNSSK4GUWT%2BbW3a6%2FrekZOTlNgz6v9Js0AzEcnNrLlHEyb5W1A1CltUKfBykA1PMV7o676yZ%2FgFK32X%2BPRmSP5olUy8Pyw7sBZ4fErgNEdGBmkcujepO2oo2h0Qg5FjDQ%2BSFzqbADtVj2izM%2F%2B%2BZxE5ju9i16jvDjposamZDfDrLx6iL6caMQIwclo6su01jgww88jgxgY6pgH04B5tAr60n6p53I%2F8fN%2BxoIIhhcG3nmEq4NtXsGACz15Wazkca3b88Ccbrcce4Ubbpcv7ByQcF%2F5giImSxUakIG0WoGu6FqrShxGsqweM2EOf8nf%2BZpc5Rm5erjYRaaH1IXECN0gGnd8i%2FWigSpIT86MO1w0BXmFIPFKGNLjdTHi6%2BE1O6ttn7fCtSO2b%2FkAtqyqSloTIsPU2H12RzaHzRg9K6tuv&X-Amz-Signature=85d0c517993326c5656f820f40dc4c09735ca3b743866b2092aaf4b9964628eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

