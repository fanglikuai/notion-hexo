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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6EKOZ2C%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJIMEYCIQDN%2BpIGH5Pk7UvCT9jYKqofJNmACizquMDHGWfF7yWoSwIhAIRyWah8Grff2z1Ee1IBgipZZ6Y1NleQ%2F9qKMsHUST2TKogECM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyli5dEnVRZIcgdO1Qq3ANAuMvIlTEiubYs1YQ0mxmQRRab98mXC1IIx31mTUTjxIRO9DSMSUYkOC%2FKoXq%2FCiL7UcvLOfjohyAiYtHqZkncpFXwCQuf8QnSTz3VcU9eBzsD5ARvoegh8cuKR%2FClhxspcjJLaTpFbNdGKE8zmzqzgW0lFSnH7xE7%2FF2b2smw8Yjn0ZgIu8WolifCAYVyxem%2BsX6uMQISWZRMhqVHgmspEOzCBEWGMDwlTtBUys0braIlIyGiV%2BiGv8zduPbbfx1csAP5bKG9Cn3Nl9i%2BdW9HJrIVo3FtJINxtTocihqZH1XmxDeNYBaFQevPvBhaLWxLylqSqZMrdGsgMlnsqd9IWVKov0GbBU5raXsiLDAOohjwwnc81k0HBZqmqMiLKTpudtRnIkAsK0ZGRLzLlhAuxDf5Rp8Ac%2Fh1%2FZ5yUYtEakD17abcKodfo5nwrxXoiDGVWe9Pjw83c2PJAon3cu5PcEUa2AfuluVnc8chpKmilr3y7%2F9x%2FkCSZCrddxaDxS0yYai6jwITsv0wIMdBgONjiTfeIpMv2fjl%2FapnTGLAx1BAhEr%2BlwWAG52UD53weAZQWGEcnHbpIBXx6nhQAiHFTd%2FTmh%2BlEL4rzmebtML%2BziJ8Yfo8KW%2FMX%2FezrzDNv4bIBjqkAZ0Mtu%2Fu6a0awDisx5EfUFrs7j%2B2k68QnPJ72df6VwBIfcrPbkHswoIoM%2F2c1y%2Bd2yKrmLk%2FeYJC49hbevUW5MJq8GKl6HFDCchIPhVZZqdwFnEt8DmO%2BlyRp5gCyWhoQTq06P1Qbs5KKHQcRmPdtaBNyCRqdmohM1rigTf3wEOpeDZxG93VCh607V4oBYdro9hYV3GZVnWb63gMPu8PRlsrk1JQ&X-Amz-Signature=d2203f4626beb4063a59166289de302fc86217d2b41fb0d6856b13f9d42fb7e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

