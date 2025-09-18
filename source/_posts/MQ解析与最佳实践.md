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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VKUMN3XZ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T120041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIESVOizWbIA0%2Bf0idUb8rKJSfL%2FMevK7PNZXRKMZqfVoAiEApO3VZgNdNV1e9%2BUlftj8z7qmefPw6m5Z9KUZgv9CE%2FIqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH87K1nPQt3%2F%2BfcS6ircA%2BuuRtUSzAMb5mro%2B0K3zCu2r8zmflzYPSvHHEv3jIGljJVSbMTdBJn71CQSigLWt9ucKLFoprnfGmgc0j9hx0V%2BvE3HmoShMs%2BHgs5YSV76gmAB1FnQ4G%2Bni1apt3WlYrWQwLcPrITPKk7dMkU2BMhth3dW2nsKQksJTDt3KiAtj0xeUG6Rpd9dTcvlryeVuonSeq%2BIvQ7OAtaDss0gPg3XG50DXWVOi2UXmMQ3H9MZCrh7i0h0IW8fpzzu7%2FfBwH%2FfGQMQqTaM%2FidshezQpDyYUSkW2cCzVm4idmsSwXpktvSoaQWfSCF%2B6EWKbuqjn%2BB0rQ933uwhjOtAL8QsccN4R3DnOMbMyFwFJ166BBeKVs8IL0NDgz2xgqtnBRARTNYV5W3AdyXPt4XpiK%2Fglz2%2FpkO%2BMJ5JS8iA45PTw0P2OeL1NHwS41mo%2BsbZ1Y6w%2FtXrR8ol%2BixxM%2FiKehoV%2FTS7TbU6Wqi4%2BXGX0mYCepU%2FeM0ZRQVFXoQstZfrtdo5KGjJYRDr1RfWsjnp0g86pEhsAXK3dWcko8LjDh2MnpN6CU66TOUVo2TjXvJKx%2FMitJWzMneEcu%2F4tNQQyyD%2BkZvprKv7zOuTj1C2TF4wdsZdB4ELOZjyyGTiefbEMJTZr8YGOqUB6SQEV2FB6n8B0E8TsVBth9uKV3ejnrfdU4DV0Sp5wZUfDYQRkXDIFomIDKQD6TM4NWM1Y4q%2B3mkYBENQR9OqfgCWouLxlcb4eUppkr2262FNlWtbjBa29d%2B7aU8O2xTGttmEI6Gn21dSEkuK8J0%2Fze49jajq%2FkjNMfku%2BK01KVgl6Jc10fGiBjohkmnfHFVzOQOYoAvUBDXXfnPPVIL5lWrJcss1&X-Amz-Signature=2ee198a167bc9a29e7ed851e9f52de0ae5d2fbecdde262c2d689d9df91edcfea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

