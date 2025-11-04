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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632522POX%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBk7mENUkv8k%2B99LF5ud8wfDnI%2FhEAJ3tLpc7dphlvH6AiA7R1aOJq5r7dM4y35V4yp8BVPfS0qI%2B8mDTT9EkFUNFCr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMP9X99%2B4yBZV7ReWmKtwDSODNHSEietHcqeeJpgFBvHKn017YIZWaaqaKfumPh6jvy3h8xb%2B0OlM20kvkLwGfx%2BwcSunI3TMtRyVCKV0Qtze7VNAM1ZloEPHgpeNfv7oV41pkntsHvqN2vTX3fAm0I109mSVCWFkCIe9Y67PKeuC6S8QFRHLs2GXBQKpcFVfB1eQpyEa%2BtiOco%2B6arX5meD9JODObM43AMJJLvBrwIV7RvsxX9L4MVBvilYPQxg4sBh13AtMDR%2FEbKauQ2SJ0UGqMjQeXfj3Gs26S1kNIcaYkVMfi0tZDBDX5cPdpjp54%2FBJQq9yMq6KzssQl%2BrzFylYqau%2FQK3o%2Fbbj0usJw9xz7ESgJ78w7eKF75cliQHZgePsUVewlV9eb0OgfwgrDrncp%2BzmsueV1vJlx10mMcndVys4%2F2XUrnk2SRJd3MmHcBRvoxudxMzAW1h4iKrIURctsbOIdkH6y99U%2FlbVt5fPrW8NvWSi2wSPk7qwcMRQqIR4xY%2F%2BEIBDt%2BLPMebpBaOFNkgrei64Rav6avT%2F57PuY8FgF2s6EZTZybrnIkq4FB5nI4m0WbM1mIfmqIesUNoLwW3%2FfQYF0iDZYx5UfYupBp3OhdQ5fr%2F4k6A%2Fb%2BGd7FSmIR6FxODPUTf8w%2B6mmyAY6pgFRMhMLXM%2FF4uJ7U3oXIaOm4pCMrWKS26DDTFddbN3VoAs%2FnewTrurvIzS27dkj71dwrpns84ugby3W1BtGTeUMbMlRC8FdzCvSsUZGPmdG9vXInyi%2FaGdr9SeWQM0pnac6iZ9AFthj%2FQlBJEYtcrr98Ov0gHPUHewugPWJfjKZVRL5u4w0BJTCoDehBXzgwOTiO2GtmfIkTmgddChGSlxJ373srQTI&X-Amz-Signature=937057cc987bd09ee97701099b22b0b14a2d2113f82ed6209ce768aa14b35904&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

