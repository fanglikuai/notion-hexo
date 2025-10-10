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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQQD6K37%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T160109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIB%2BZ8N8BQGMs%2BD6rUktrJ1ZcYOaJ09SAo%2Bn8%2BSaHW16kAiBXtU4lAQhCvqCvLBK5NEjQoLT2HNq%2BnqwkkqhIf%2BI8dSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1AmCIl47vybo3IzcKtwD1%2B1FdqOsF77DoEtR7jqB86wzvLtOXMyK0IBUwPyZug%2BxPdILEVXg72g7sWbkosYlWskOFFvImwaagTcuZgXfmlqWDwoaY%2BvL0hsI8NRk2leDks9Z%2FHTFJg69OB3Usj19M9DfUZKSy47PbkCvJ4ci2E8l2YIm1CFw0XQFVmg7rgSOqy2W5x0NYAbk%2FKDzt%2FunM4LLmqORjkSt0EA2kqKjg8nBErg%2B6%2FJ1ujHAcBmKgBgMVoydMXK31RqJrz1ypVxoPr6KjUd8YXCdX5Q5TE5Mz%2FyfL3pB5VQiR9UHAK7wtVKH5koE12lXrjStvuwFfWQZyTeD4gp77hxDgIztqxOBg%2FxHaDO%2B1IPKHUUPK0Q7wJEAqTRCRBMvQSbwIczB3Sf%2BhMdCLEuY553xHFFnehEWSCw2kpiwK0GzawIYKLdLj%2BHb%2FhMMEssCQNlxSpFr9GyoIuoC2g1nfX1XNVKwCRl8ochG7ca3QEvcrxLyFNs%2FIUYA9pDWOAKvJowKQ4e7HfJirVbxhJad08CNBizCqIAFji0q51QTHGcXZDyMbTVsFaox52T7Owx2vdIhfKBZ4Y9LV0h%2BraWpjC8hv3tFByFXGpffI98P0T7ldne%2BEbBSEF8QsuHawSiMTZp5Y7Iw1dqkxwY6pgFopKtqdYeYe0ybe07qxQede%2BnTsr8onVIDvIT3hCeQIGPumrGys%2FNdgBfWLBq68u24yhpUPmPZE4EBM8DryvURuDI%2BHqx2W4PmeEpriMKV%2BddiiWIWZhMogpjPO7wSqKr4Om7mRncMXjYIaMfZrkK%2B6dyXMtYXDZWxiWDwYQewW1mGuSlN3uGSGT48x65zJnb0okYPSavuRLn3voU%2Bri7SLYtkUzrj&X-Amz-Signature=20192e66c1a3a1d252591296fff8496bae8ba585447aa5e5b806f10f6c661662&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

