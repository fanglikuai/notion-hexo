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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665AQKFEX4%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIFgPD25r%2FO3Fp21qH%2FcsMx1N0SYQOaAIHwVx5JBEMyrNAiEA84o0ZeLhEW%2BYDpMekceXCkWfZgNv1fuGZIq9szyTkesqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMRzFvTKZS33YvoILyrcAwKP5lpYtd33ffZJsGCMY9njg8kK9FVDlmMFVMWezJVlS6zrYve9bYJQmH6RlkuXgjiLKXI00ad2VE3oTYpUMJFwvkBRYiwiALnKxX%2F0qs3FEw%2Fe2QbtvLs%2Fjnvr9OygDzsqCRabYzPX4OMEGtaV9s6itZsksqRYxBLhyGAd0EZ7Ijjnky2khRX4E6zNNe%2BjTWbNI5ovCdqJAPms%2FeaYRCa5JfohG8O2fDadDsB5gHwbKTqia1H%2BuAQRzyHrx8ZMg0U9CEuKhI0VyCYwYDObhpD2JapLLjbHQYOxpfOrq7%2BXm1Py1KVz3QkjkVzTAL91m%2FpE3vIW%2FmSC7cgP9zNuKSk3aanIWBaMvGmweJkSauHhfahNLo0%2FkwoNLs0UPrLz5T0ZR5un0T4O8bkXnkRalRv0y92lgftBR6dkJ3ePts%2B6hhYf9MRnKCjmfvXj5xDlYA19mAMlZyx48yFJqLCV%2BUkUnCfw%2FGFP90o30nGJwdWLloyaL1snOHlD%2FjDd0DRnSs9Q0aJGh8N9XMGvQ0Jn3BQ5uiyAFlYg8NZtgAQFggMJPLXsOexiwExmvQ4nAkem%2BxS3KmCuZU8tsqZFLx2rXFmvWpvTEcwkdyfWeY7a%2F%2F0ZnArKGabzx1bWFn74MPLbp8YGOqUBR4MrzON4wwuDDQ4KGXUSIKPpIcSHDqLi%2FNrEpV4nNdkCzKFc%2BtldySJZb3qqXsKqXvaEW6s%2Ba5HeJN1rqg77AifOmXfJfyCbowCbOqckpDVd%2B97Pnz%2FLHDP5CRAxZxy4rSwEvd5cebiqyZRaGk63xR2JQSzPIO74dOHC%2BLGVLVpcYFvsO%2B8manfePSn0hveObGquOFnO61FoDR4upNHRwpwweSWT&X-Amz-Signature=6cc8f883862cdbc280bd84cd3fc6720c8674209bd0791fac19e9acebb45c468b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

