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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624QKD2IH%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T150059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDnMab2tQWQHw3vRObY5htJs8mUKLYMdCHhbzfAG6gxYAiAZvOBUl6p3EqR5NKzyVioV6M%2FBSjkBmpXqtIB2e0lpbSr%2FAwhHEAAaDDYzNzQyMzE4MzgwNSIMFxMd5xMxwm94e0rlKtwD8152f1syQvSvkbH4K%2F7NUakhmllFvtV%2B5g8kIHCmtTjMv0tkBiYbOu5TFWs%2FEZJ2lf4VQ%2FG0a7AIC6SebZCXJqHF%2F%2BjPXMbSdQm4EBbuTu%2BwtYa%2BoTzNkXvCE%2F9SQAKrJfMFoMvSUKIKKtM7EtJrumLpncw7LSwQgpszwSQQ0B4wQ8YlMhpJ51z70UExecshpKOpmtw23lPHRqZDuczrLjVKdnbwE4SB4R0v%2FUtgMGPuXK1KeuYXsYqmdRuCR%2FRugD4AX8BjR9OomCpeam2KGUuHDiK2r8PHI3aUrf5RpikR%2BwAF5j1YiLM%2BQaAEtipAaphHg924qifCQd%2BQs6Xfxn5qdg0CSiKFg5UTm6xAD2s%2B%2BhS9dIUNs6M0ZtYuGgMG9fYj6aBTuSc5ZhjO5wvJY%2FatITpLYXs2%2BTUA3Y7JJNjYxy2GVETd8vxwkhNkNCrTE92sK7HPGghoanttIisdfxdUuf5yPEwTlbd88pR2RagZszY9%2BYINw1ZcqUraEOLp8kdO5OhqUGG%2BPhwf5%2BR5zDgNfUG7C7u4Ofx3rHpEOCi0%2BpcYHC8yBpb%2BaHlAPCi%2BpCESsL2I30iZhkv77RPPn71AmYrcRdMwOvmyMH2mImC6rZQpuq%2FtHA6WqeYwopO0xwY6pgFmXxk6dd22A98ysCXSawNsH49nGww7F1y7SZuOwAWVZkdFDCqDLCNc7S3BMi6p1x5odX7T32xEV01TgRH5VQRMEgdHSLQ7UlHnmSwbJ5OjYIaaT9kOLlDOufEmSht%2BUj6ETLElEcVOTlxTL8Sfr%2Bq9ACe%2Fjyk5bhHUN4j8neOpwxvN4BSxkiCPp2IcdIiSy0dXEiuF4ohVY%2F1MUGVq1Pyjof9J6vLN&X-Amz-Signature=d927dbe49b1213d8b5ca38c09a40f3a2050fb3c14d01282e59186894c9f54513&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

