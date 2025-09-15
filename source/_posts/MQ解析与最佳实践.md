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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQBYYAUJ%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T090019Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDz05JrteQzegC%2BUErK4rMSRdDg4ZAmBnpYiH3xMIA83AiAbgwjhhcnnCk%2Bh1%2FOrC06LKYeBHsNpIOGmU8F4QwjsrSr%2FAwhxEAAaDDYzNzQyMzE4MzgwNSIMqn1cPql3IDM%2FjiAfKtwDFNhv5R8y54JqkoyjCd8woluhauSafSVrtzFVc5iGV9hP7bQ2Dlz7huPSa0aSaZ9wsvHM3ybGqzDhKrgNmOmvQHzm68LYaAVRq4B%2BqQS8jjNwwhQPudH1WNOya0rrhQMfW%2FLYvhW14dJUzSC09GCFdFJ4RpGO3q43NjLsFB01b02i%2FIXG3dYj1NM6%2BgPKp%2Fe0Dm%2Bmi0kUiNRdopDFmKysLjrUn1U2DDWxmf88XVE%2Ft6%2F8BGgaJdSo4lFOrWWbjfl5MvCQyxj9wqAb6g8v1k1sHgagldSE2YlnkQEpGUrSLfutwgub7NSa2l3Rzf04R1pfjEHHGA0TeW7%2B53irkoscbN4R8ryxgX3%2FpuRsEThl0uxP02kmxGTfUnjNludMbY%2B84Uqe6UZ64nisWxwkoGBWXdiy8kecfKLzym5KI%2Bf9NR6P6WZZJCPwfXqdYG3vCSzAgpyj7d9CQa0j5ZfzrRRbQV%2BEubVgO77THg3vLQcodrp86%2Fdh5n0cBa59lmEUQOhHzhSdEqwB6y70UnsiQ1Dljfwj6W%2F%2FKVKivOyt0BgAJDMBHBa9AnZmfmqhp3YWd%2BGGRtw6q5gA2wXtCQgOMBaLPPemcfKRqqCrZO5MWqpzot6s2hdTU5yFRuO2Ixgw7pOfxgY6pgGPn9p7MEkNZ6C8WKs%2BeU%2FOBRhuU16MSgIDWfAE9er%2B3iH9KQSIyIAsnpfsdi5%2BDcYA8XitTgV%2BfV%2B5DEykffk5DR%2BHqiED9LSpmJeg90mnfaYQNow%2BPCYL5plu1c745eQ3gAyN%2F0aE6mYxe5jikoS62lHoQ2Og9sbXg%2F98e9E0DXlm%2BFrxQOSesw05nq2Ns2SPy7ycmq7A7SfrwRjsuaz%2B5IDd%2Ft2e&X-Amz-Signature=6f70951d549ea9b83dca00a2c7d2861c3223e6a4c8dc847846871f242b1005b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

