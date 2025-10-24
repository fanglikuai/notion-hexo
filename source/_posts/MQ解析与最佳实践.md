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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UL5SHCLY%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T130104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCja4eIq3nnb%2Fe5Yowda0oHWqVU3g8XyLh9vusVjGj5qAIgPBrvM34BpoxYIRw3Qt%2BPqKEfstqkOPV8yJF34LECqtoq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDNbmttUBS7OkLyzYCSrcA7xtwzxbJuiGwj8OkEzYGZ8myhUaFYIYw6YR34EzU2uPinIJoetMXH%2BE31B%2Fa8qaUK%2FjyT%2FFUel2YVC8LsyoUpW1tqipLSmLE9MzCXjljYBB8uhLjGZJzaGTUcuWMmckiBEy67zrox8vCvZQ4OkGMgrDSQZ7nGGY5nwp4Q9ZoKAkIUfVkjUU4pkhcKmFEj%2BqmV6RFpYzJqclrewIRn3a5ZnfoCIw%2FMypdRtYj8Nt3cS9%2B%2FlrczkHaQa60yzhew8pCqn9782hqxdQyKmpXFqffPQYnN%2F8jgeOG2d9aUV6%2FwPJXPkm522FgFZJxlo2bZBGrdl12AdTp9wlWBljeZTiKxa3A%2Ft6GZh2M9zOCBLXNy1Pt6jDpLveptpA2tBxeNZyMYMXPOwgCfFmQXJZkbsAkbWtz0406BNHMQYl9b1SY2FsXvNbbstevlkMLhVXawhG2OGo%2FCS2Ofz5AcSt1m%2FVkT31nHm%2FKLLPLUYWcjc3xi7Pkq3%2B%2BCPd0HVn4VE2Sojm4AwBA9EDsdmT%2BokOKOrk0loLWwfQpTV44JpY7jhBuBMCruDwZq8%2BXJt0Nx%2Bby9vrYMga25eebC0m%2FDeKysUV31V7kdYMlaywFnpGCmM48dg%2B3ZWyu8XEjvttt7WtMP7m7ccGOqUBzAFVBId6dQ2sb%2F9zwvNYl0ZYUjAEeuNfQ0Qb%2BRGHPmoCgwEjSxeqV9MN2vr30ZGUzBsdnsr4zmxMkNOZHB2rYh%2B1eEaT9i85l68O9UsaW%2BzBt55in8AdJpTzuR36h4yo2ueWJARZcDisb6FegmFDCR6U4L5iRBrcoSUlEKZNBHSSCx0k7uL5a%2BHsAGjGFi1r3jqY4BdsJ3E04Y%2FcM2mdDpwrLQfV&X-Amz-Signature=4e986ed9871703d28a4447eb3cb4c84958cd6b568ea8de61835009c0385e1fd2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

