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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTJA3NCI%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIQCVn6JNTgZIvEsCWlL%2FgnfMKo4fPMm3y9qtK35XZc9K8gIgNZvTpVzpNygTCdWo%2FpujB7Nkup0hP7ADRq0OlfCN7GkqiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJQOlr6UQEvC5ZCbnCrcA6KOj920eG5tyX91TMA3TZuZ5k%2BogiWZtWh45evXPcBVCEKUrR%2F33Rto11P%2BFUZKBXhZG6pwUpaZ%2BPLNIMNSKj4abhZ5iYp4wMuOqOW02BCcA0uODnBugILZgdJ%2FHi3f3z9pCbs7X70fKgcHcLOK%2F5FKqHfJb1wiA4blaAChIR%2BJ%2BngvsuHAJpafwBjUOTy63lU66%2FizoOKtK4EWvCdgtOZ0ywkT%2BG3w5DHckuX8SN1PT0uyLiSNSNnKOV%2B7i%2FyLhtizJM1rJscsWJaY0CF8gT6cdEFgE12PEMsCWUnK2PyeQmgmG8XvrcYF5E0J8x5dh54NZoUKlLOWauwMsGMzeS3xXYyN%2B0NINMJYJruPC%2B01jsWBGydTMlAYhX6DyvrG13b%2Bvq8WIs0yiO%2B7YxsLY%2BQJPpTjNAEvtjw91FXkhZowmRBDC2pGMTj3ThWz%2FivuSTJd21dIOR875%2B4dZ02biLcWF5YYVENS9iinQdLU0mHQcSJ2dzfLAMuMmoz4dQwQ3mAjxK%2FhVOOqamNBVC7jwTBzgx1S5hsiGj%2FWJwxaYYEHX6FkhkY0MR1bV4jVUlgnK0%2FXeE2HW1gdhttIDLhbnhp9z0xGZnl2tTKfO7SPnYP0ii5bxaK9o5AYQAt1MKv4qsYGOqUBRLSfRbsx9q4fm6M26DSWDuNRsGt2tL5VbgthlksXH%2BunZUu5YHAwxHAmlHgjokgxM1R%2BTH5ehC7su87HXDM74%2ByT9tfzaDQARZvck9hKzgpkwD%2Bg74aW9sH%2BbN7xH1i4uTjyttNNHq8qed4m9fU%2FYhPrKCoxujlRCyCrB4%2BNUxUqTsOi2A%2FzjvCEGX9TOTc6oB2Mx19xKnVYvKMmyUOAhOLEQJzv&X-Amz-Signature=10326f756a33bb5582849d3906fae4e95b2529962821f26852741153f9f622cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

