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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WZS45AD%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJGMEQCIFPP%2F9GNrzHd0VwtLFC0%2FnRuukCzzYXtkkq4R5ulPY6qAiALRGLq8dRT1jnL99wBrD342nlCQnAz1UD5aOTfnW%2B%2BcCqIBAib%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMd%2F%2FcPwTLefgEZVtdKtwDevb%2Fw7WJYDzqqX3l3hSl5tXJpQHsRMJatu5BnDN9WPubx7UBcV4IH42S%2BsYaEfQCYeieujNjpqoxBksjLhd%2BM6UHCv7SN9pxnvN4D%2Fca%2F2TSgR0a6oPD8MeOzepFclsy6Gii%2FyRhQ1D8VU0BImdX8fwXtvwihrGSRJDy3NfTjezGHYuE%2Fj3%2FMHAWut%2F755DXhzq5ybfrtG3QcAtA11a1JQAwyO70n2X4L5pNHbv018nXDU9XP9Cf4bCFIEXPylf7VOi2chgA%2F8qRob%2FKzuGc3tJMarMeIviaoVUXUqf0H7whH3etOCgjjKM1RCkVgv3suJoY1Pvuq7Zu9trSfiWKXGyEzN0O%2FOgVicM%2FPnvhJimBCqMLEIqzm4BR%2B0Xxxouy6X5Rg%2BmllUhHFOlUltkVopGqBBQ7NW0T%2Ft%2BG0j5E1q5mr44oa9NNhIPx8KBBE3dNd6Wgu%2BN5O1qb62aioZ0SWDnYbPFaG%2FTBUV%2FJds%2Fw53TktzznwuG0V3dUfdLTiOUa1ZkI1hpYUbmbq4EMPD9Lbz%2BuNJ29JPRgH%2FdxjtnnBRCk5Pf%2BgkGZBGaRexixpxg7kP5loMEEYv6s5gLpyqZU3J%2FiOTyoCJVyvoaKRYS1b4mlQ4RqCafMjJL4kEMw2o%2FdxgY6pgHrIycb7nokf9eoAG9vqWPV5uCsgtf76bVVftwjxa8IW8IOrZzCDCL1zlObXJOFTSZVyasErmipk9WunAw3Vkeu1X4axUgylT7Bu7OKijZhd6TnTDbLiaddoXNfE3NuAho31eQUfq2JWq3ctF3m4cgcN208OR01SV5zmFQGsSx8PYBcSfk3p6o4%2BLerGBNu3Tz4pGFqhoRt6%2Bsf2x%2FLsV1SDuzcWDt%2B&X-Amz-Signature=e53370f0852aa79ce5fa84510b09ea7fb11c5f4d932c291653a49acc51a7e947&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

