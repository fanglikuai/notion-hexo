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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TF3EANFK%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxFPzEOCKccph1Q7kwdPXiewWnY7O0PV69J45RO6fJVQIhAO%2BEu3VJCYh70hWolBsKBdYOQB5vcfIf15diOjOv%2FAZjKv8DCCsQABoMNjM3NDIzMTgzODA1IgxxRfPyzyudpG0Elvoq3APUDN%2BpsyKw1uNlAVYzJ2RNH%2BuAUeq2IcU7jjQ3mClvNoKcHl43VolR2PKC2WsTgfgqEpbSE5oTPz7Bbxf5%2Bl76uwYFRKd8Q%2BsIe%2BOGRelDSe6ZtX0SlzQl17pvMKzbyttJIpVeit8nLUMagqUviR5%2BSQGt44%2F5Xdj%2BnL%2FoEWEnqApijEqFkcz7dOzz5coONPU4yO%2BEhDwGPewf6DSp1WwCnJNgrVcGT5KQxeTAM8bKliLu9NZnkj1F0PliiwncEl62eYiglvoSjWvk7cDFWfPjnPU2isVZroLQShaOGOH7mTmhaqaJTCcW18RNDXi%2FzDbokulLSdvp7f76K9ERup7QgmUFKEcOD9WCuna6JgfPyZNBlDpyn1CeH%2BJRU1eSx%2BINinLedfTDVHC%2FhwmTtT5r3L4qjO%2F9ofXjvuxlBdGrOsEEekn%2FP%2BE6WzLRxSoVDKdWJrk4IRgCNqRtK%2Bf0u3EZuNjLIJtYMZxQitnk3bM4gp8DqwtuNGDrBd8dZmcXne7TnKj8XBtIz1sCXgAyTDTRlV7cRMsOsOnx6qOIxk6IfRsUzOAa7xI83laa7lCVPDZnf8p31wd7bWw8YBGT4T36TDq5KWxyOnaeHFrm%2F%2Ff4rRwG7cU4NNxxpsda0DCv7q3HBjqkATDzIDSWmdz8D9rYxzyCH4Qm3AhcKeVHpYSnQqrZWH2a40%2BaJ6N22OYtHwyMD2GvZ9n6CWQX33GXP9aCrfb9lvV32skdYuVwqf17CwkY6EcXl5ApNAyhC1cx5iVD%2BI5Zlz0lM%2BmFcITH5qEfO%2BHAHmocCu3WU22DN6iLMicGwYoYGHhGeXGQVkYfPfrTTWktVQY3DM4SRsAm3vfvxPkNOGYt0e2b&X-Amz-Signature=facf0bfdd223d255bdee52a7dd59382d7b8ba707bdfc74fa151a2c7c9df87e2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

