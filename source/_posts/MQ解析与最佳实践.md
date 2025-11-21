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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCLNUJYM%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQD0VAze7xofzPAbbvyK%2FS7L%2BJCCaddzzgccEMOI9Gz6WgIgQqtzKQhjPL9TpbPGJ3OqGOVkDzE3xhfmMuWgleVPRLYq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDGZ55Y%2F7656ZTASo2yrcA5L15IA7cdv%2F%2FDXXGHxsioyRSo1Oj227178qJmN4SeqqOqvcZZ%2Fv8rGM20qIC6LPstCELdJYNSDpmGV%2B7Gl5kqcBR8lI%2FJR1uABo4lCR1UzdNbpPPICkAEx1Ip5lkBN5BwXmL1q9mDxnbmAVUa8jRtAaMV9IK%2BYMzg8deED5%2BcABxL7oRz%2BqQtknJTEOCUr3xstX%2F%2F%2FSWpS0xypNFPdSiDrrH9%2BsSUJyKWMnscUV%2BpdknNkfFnG%2FXCHafo6VlJBcBN0iBIcKzpF9IuNAynrJyJenvIGK4w54VsapUR7JRJwRGEh%2BLwUD5oP3nDvHUolKhqRvG4BUTkjSbbryqqYt3agatCMSnnNaDeV3S1Qws8xRDSZARyfdmQS1DEyGJ2CECG5RLyYdDLhzixKmDiwFhQfrwuaIW5AhOMXz8nD0IAdduRK696uCRxjRi%2FiVoc8gNFJMZtBgkJWiVJR1MT%2FtS6SiulBrfoFCXQToHBNiCBjQAaQxpfsUH89lnJXbdo9j4mXuqMokW5DoFfyYSV%2FbS%2FDNteRY6DaU4Pg31E5BZJQSAOZwGRd%2BGf45G7BCHuSHEaP2AlyWbnlDMr%2Bp8ECjj%2FXF96jNiOjoUk2CqY5vGpftDvOWWYpK01FvrK3cMO6xgckGOqUBypUzSRS%2Fxh9YS9dBVTAOUPAlkadKL%2FhinJFZ4A6ONYr0taTWIAUDdsjnFkSaKWdTxMMBsQaCGJZHQbpf1S6WFCI82jHgSuaGHKG923LXU26Hi0vu30xkBwY8q%2FCDgcg%2F1ConrZjT2axTOWDUyZZLpwIPntyPiowiYTFuQKH%2FtKVook1lk7t%2BdDy3o4xQL%2FdAmKGzd9MzBf79nlWDafeZSxM4hEUW&X-Amz-Signature=b26205725699c2fb8c03710a10a13034a6bbb3c982ca7a72b5bbd5a80fd13455&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

