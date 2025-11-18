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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643GSYVCE%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHqkRjLkF%2BRmj%2FnRYiH%2FbpmLxXiOjg9f5JnkCT9VoJhvAiB5uUmf54vi5XXQlZcOK7JdiyRBiZXVnAJ7AUGE%2BHvplCqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsi%2FnELsbsuD4MxTVKtwDN%2F%2FzW1LTXJoC%2FqujJcrX5uLss2%2FEOGFCRRuVKSo9S8buhtD%2FEWUnIEDcgW7IUpWendSy1HFzCkDD8MtYTsPz%2B6s3iMDddrjPaa6pYDDsYoKp4BgZ6l%2F%2FvbVBHIZxJS3R6aQBc%2Fh7cfdEL9%2FnVmRY2u%2FkX1vsIZE6w379hNlhV5jyoyJ9LS4IP9SIChWnUoBwhDsxcez2eIq2p2sW0gmQ2K%2BjuuaWaHmEDgRVc205mGunmvda5E7Ha%2FQc4LyzXThdVAE1MqbHurTqdVwUjf00IVfMTHA1li9PE9%2FOzkpcrKEaHZbmi5QGErRXVrcNz0OCZjnoW%2BPWHWtbXjKpNBRLo2Ea5rWXtyyEM%2BnulGF9t72LUwwCqb1YjkKx%2FAy4Ac0O7lQcfmMVCPdmrCaENHOhFsA8MFFYlQGvrIdoKc%2BFvACcnGy5BKC41GAefnVVoeCUPYOH6fzkATbIGzKrT9sSdMAiZk3msll8YGsHb3woMJ9Z4d489VRbAuQrrI%2FafdQnmW0DCUPHD8AyCjHJFTmFBAPnTrzoD9c90okgUOVikBNIL4hvnWdo01CU9N8PqHHfgZLOUz6T%2F9aqMl9ziPzKN%2Ff%2FyG6NZUPRBC4j5ZQFnAOgIp14guy4B2pdKVYwl93vyAY6pgFtofXoNCXqSNp%2BZCejm8tjUXYWmAQE1GQ5vSLcFTanf999beZEy0uTKwzftjLl7lS1L%2F1nR8LCMmOnQXVebGZMR1OYL8TNyTY6k4ppWdeIpiLsuwgGKAjrs07UQ%2Fh03gUFZ2EuyvJwkFMvFmDLS71z%2FXJKtx2%2FFDhjQ%2FnM8VwrU5dtJt1eEsXDagVixMhq%2Fiz0AKbIfu8BNXbtfl9%2BQtcMo9ES7UfN&X-Amz-Signature=b718f3cedb2da844b7656116b43c11c32eb1c77993029246c0b695ef6b71fa70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

