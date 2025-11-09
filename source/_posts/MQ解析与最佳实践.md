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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVUPOMMA%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T150103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCIEd97A%2BluO240HGX6lO6aBCSprSCWKE6iS%2BFT7S%2F60HEAiAI2SNTHts6kcx8b%2Fj5T24jVQ%2FgWHPUA8IKLHhdQIz%2FxCqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlFX%2BQOamDiUSQmGbKtwD82mF%2FJoPwEU%2Fgvo7IAl4fEyGrT3kn4mNWD1%2BMlgPe1gPht%2FrR8%2BkySap931dZGSu8IglM9zkx2dTsZDqRy2uA9%2FgiUX749Birmd5ZAz%2BM9XjfVAva8dp2Dez52MoUPhHSmIAA5JPv8puOCCFSba6DqjuW%2BCcnVRLayYEnxUf1JXqeUCxdf2r%2BaP%2BrG1me5MSPOxgf7GeJzKtS%2FUkObCgef8iO5Z6xpjL1DT%2BF3PdJk4XFXDLsD%2FqEScD7E9yd1QDfB5SEm%2BTVmQbQccUxwsZgI2tHdrQhzBb0zP8jVsb6Z6tfgeln6OjhJ1VNtCxbTGTsqisYR5doLzq9Lgx9yIa08v45Vd1gDnAqbSNnpTjkHRjEFUfbyV6h4YB7X5YfAsgeaW2%2FX5RKdD2Tg7InxfTOLAgP55rJVC4BvE6u%2BHwC%2BK0%2BlqAePHmrN0w8ZUcjgr7AOReWctvTl3d6R23sGfTpDCUMySFPKc6pommRz4ehW7hhtu3DYIQNDQj01aIwfIn8ko0P7KyfrFclurxjgTz9tM2JL%2F5QCqtNcrkXtH59fhTuIiS3GmqbcRkgVNIWdJ3sJ25FrcQNVV93FP4V0DYzn85x1x8k4Ss7dnmIcZvLK3H5Dlj3lm0wJsc%2FBEwmK7CyAY6pgHdseNyaQ4ghKgAI0CaYgvLxpzRwE9Qq%2FQxwhtBaZDF8PpsTZc42iThZ7GG%2FpNjE6%2BkvsHvXYSBaiWzi8DCPsvaFh5bfFZNFrgZbPcBZBoUBOBHNIYMwMKH9yHKPFH4I5TD3VucAQzGaSN2mWJi7TeaSsR%2Bu67FScuY9Vlt7fc20XELJ8zGV%2Bb4tphZDK17mpnNohDt5Nw2ds%2BitcdjeCk4jbGH7hwm&X-Amz-Signature=31f09f57c33927e98a02b9db21566f9e6ebccd3d3e2912c05512cfbcb3a6a441&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

