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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHHOA3FS%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T060100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDIRUB8nYZYEyKRVZ7HFGfRArB77kGD5WoLnjqX13zdogIgJ3UNSqiA8a68DNYoadkZIzO8zrJctQwV4ClO3%2FjfAmMqiAQIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDESWESoSG8%2BGS%2B8hBCrcA1U4Pvq5YYUdmRRxaDXDHQk50IWcleLObckLL1%2FP3ewqKPM5hcq3ji4jHypSQnSQZFF67z2g%2Bxp%2B5ckTxn1Raj9khTQM89SCmUnq%2FKgJQk2q%2BBMokuewy5lKJWp%2FRiNzh8nLJ%2Bq%2BjLNeHPGgcKIdQuJkQGuou6u0se9yBJj%2FtHKkt12tlF0EMqi4C3xe8qKtoug8J%2BhipiVn1%2BozE42%2F0d6MmeKqzD91T6%2BCtt%2FKdEWeolProFDKUEvIwulcEFfmzDnLnQBKaIf1M9LVvOCv3uQuaY2VOajVqjCftg7oJi1gy5XTru3M7BmPzzV%2BO1e78t3pzOKPDgfOVfUCaa8RoMcqC7TEbwH%2BerE95FN99%2FCVzku5EwOG6EOYKt%2BG7xdMpMSuHNElextxRFPZtG35lGThZ96TqSmoVy5U4vDd%2FfYO9lKTq8Yg6dlZ5%2BGb2oeKEVZkdFgddUEGKVS8kTzJsFjrNrcTuE%2BDwIiPLYpha6bXdA%2BPkIp0utuXbQzBCDXckNCi3UbljQZHjJXbRdzgZj8jNQfj9t7%2FLj%2Fy9WtJkXG6Mag52aoWd0ZK89i74q86zrfgafAA1eJIDP3u2%2B2Rn7NT872YNAMYtEvhT9AZRdn%2BGg1lfDY0AFpM4jhOMLycnccGOqUBOOvS5UFSTebw1GCuZJoXFzDBEWBZEIVoQYK9HKIpkT25jxJzv4NYxwSsjMd8qCUfxW4NxjVmS3K0EfN7HKiR1V4Ko%2F0IjoV3CKSb%2FEIaMSlXWaQ5cH4SSjtNwtMfI1%2BIWJ%2F3%2F1txKNv%2BIE4G3DV3Km9V7zKkdSw1utag9txL0e%2Fa8ia9AnhbCvDSfrAbrPzxwstr%2BsbBTMpsvagNLoetyww2gPrH&X-Amz-Signature=aa5c8ff98bf94a03d5835d247ab4f87f218df6c7854a59011183a84a610f15ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

