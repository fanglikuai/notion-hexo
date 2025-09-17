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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XDVFXR2W%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T140046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJGMEQCIH0n7%2BVm%2FAEkP7sb0MWttZ2rcg4%2B0bdIHRoz5owTC0y%2BAiBS%2Bzw9OxlMrcBaPI1JrvLjhnGkLTX%2FdP89uTUBdLxlOyqIBAin%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMfMGGF5uxG7VWNfAfKtwD6bL3Gr7tFnAbKDI1RZH3nJn8Im3wGytmuUaQ7oZl7PN37YY1zv5dMMbAIx2krr0CrShjfBzQrEfT%2Fh2dSkptSM63rrR9iGOIBr3hly8Vcw0KTCE1v%2BQpEaXwDX1GI0oZ3G7b1W%2F8fp1xlNGKNIusklhQ2kPJf2xCpum9C5AA9zjjD4lRtM7EwKVSSFYWBZdP%2FUI8HmbWlK8t1XtPghoYUzBa3%2FhqkmgbvfRE9v55PgOVH0jLUGvHv6hVLJw7%2FQgCK47LG0HPA1CbvsEqxSWQ3Pk%2F1xLGm9MfP%2FwWouO0M7zZiK2YRlkfbRZ0bYjTG%2FpuuJ5ehuuj0QJUd%2FsDEkTH2nNTi%2ByYfnamGJVxVW4PdOHN5G%2BkRHQQnIXlkt8ouijk7vugXdmwbFTs8RSBPYGC85046XU0bdjk24Y%2BrNjYETIDDec%2FLa5C3uhxHYQZoRh76YdoE4SQUciJnN1MENBBLx%2Fa%2Bxlbra4uMXVowpcZpn%2Fz40dFLN3hz0T31oi2g9r%2Be9xNWAY9h8mR9CefR59QKeAWdUD0ReeLvLUMwVVEcdNhJWL%2FkqGzU9NuHFkKlz5N0ZGpxb6topslMJradC426t2FzNVedXagGCc9QJ4QkapU1ssc%2BoT3r%2FRdZAgwj%2FiqxgY6pgE1w8XyPuaQIByhhWcxG%2B7unnMi6%2Fn%2FvmhKclOO%2BXLaWsbQ1qZJ%2FMdcdC%2FYw4zpMRVPoiia9BqOTEbeByJUQtHRYb4%2B%2BX4ZOmwnpnyGKX7A1uR562%2Fte8eBeZjs4SKcFi6lgpgyfoENrj6O%2BplVslwTuaW%2FLOLVsJLBVWDqk5ofDgTbMjnxOqU%2FDz1%2B3nNVutOVuvTh8nceipgSsmGbkfrEm4yz4Sw1&X-Amz-Signature=0b0d662a81f7ff2be8d5a1db688cb947ea5dc6b5934d5223a387b5fca2f8605b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

