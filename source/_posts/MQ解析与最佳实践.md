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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLWZC5KD%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCMWhNqupK9Rc3Uoh80BV%2F6FPs2uDHGC66jyG%2F8hmKDsQIhAMChhBB5Ev%2Fi1WtYWxXF%2BmvPuLXH1u5hXUVVTDMe7E8yKogECI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyexT7TPfuWM2%2F5lRsq3AP%2BGdwLIzUPDhVnDiZ9rkR0bKfsFz29CXc7Qc92VA%2BeDo5x2V5f83x0JLAoDh1OBnN6TSbxJkLh6eeZa1Ajr1UHrZInHlPWPHDmHs4IXJPIpIjy7Plj3nbEd9KXJGOTlBOWDCSuIZyYT%2F5VY%2FSvKEn1NRGUiU17PieN%2FjamQLD0MfFbkf5eHzaQvJnMaHQvlr7qe5yvix5Hd%2B8jr%2B9qv5t1tQjIkEtH%2F1dTFmlJk%2B7EC4shz4dlDagjURkTdZc4xodzep5uTmujouCT%2FIjy8CbghKLvZTFhvcIUaJkXcHD8cUnc64PXSXojeLSWNzRc4RezT1M9nJvEcXJTMd20tK3%2B30ix3NIG6Fo8fkZa1GsU3mUWsR5YBgPTtCb37nePDjB4rOE6rte7%2FqMOIJUBbx%2B4CgoLlDtNZPpfVjyNN7WDVyWji058NxEDMVMBRaY1V9S706u%2BWHqpKV2fC77q1Pbwy27m%2FZgtEmieH28%2Bf6a7j3%2F2Kpb232TPM6rnSktulsaBKtJQrv%2BzrnGpxYT60Xmp6nWCxeg%2FcJatNuzVEHN23f5nCkyMuGz1xwqG4jw0XVaE8VagWMUiCctqgaRkIO%2ForYtNujCv5QUhFC0ih7b4wBrZkZEADz8h9LlerjCb2PjHBjqkAVRxf0z5BZSQmf8MDq8OUvbDpN%2FXmXO%2B7ugE6w0scgvyfWexU514cXFqlRkcw1d3LubEOad8rCApmmjHZWK5lNKU1hQnyLwyH%2FvgY5aNSZtrT7exF4KBZVZ2wmFUh6xO9j9HlavUv5C32wm0Sf%2Ft%2BeWRluG6ilIHY6ZOR0YRK7IUgVejIvSr5oXh1DVleY8aMf4QZuj045CXB1nbFEJkhcsOWRC5&X-Amz-Signature=d5376be9d286538a63b2d179cedf05e467f5237b750a3373b1c1e0b26f2cd119&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

