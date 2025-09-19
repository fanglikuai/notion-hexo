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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHIS3KNH%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T010038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIDiMydSiO1FjKUUfGZdNoJrWxTFMADfjx86pHO4QUjd5AiEAvGuoGKFTJeH76MJfsXIPNrPrJ90gRVQKOCeIWiCSNTkqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN4MXziLHffyFmjkRCrcAykZHzspC03DTISpOHiIYsFb1hgA%2FA7hZe%2FCtr0I6RxmxRs0ZIs9QKi%2Ff63ghSnyUmcc8877gL0ETUmN8p82xiRGkafzSnn2UYucBEkZKu2rSFy9U4tMCOLzzOB7HBQjfGLLpe3EcbnsJ4IUt%2FeEZ4Iuy7qr3QpVRRmxY%2B2t0V8GIIEozNwrbyVuJgRlS81eVCFvAvTBkf1HWvenvHt0sj3%2Byl8uOqLNAGu%2FqnUu8z0afBKUz7jWudRyRL7gcy6lfsu3y457K5aFuxRQEDUHpILqOPY3A%2FDwMZjOHQpUArUusm9Vlk9%2Bu5SoRnWlD9BhRwLk%2FilbjIW1yUeXlnbbh%2Bz6iea5MzMBHqt0flP5xduxGLFIXwzvk2l94Ng6fPa%2F4BSn6hp%2FrlSY%2BaW1vCk8SQQqy69%2BWf%2FumCe09YQvu2cskS%2FT3y%2FCoZ6Ez3TKduKOOFQwJ5FZtz0r3qDz6Idb2O3fDyk7HKUNepQ1u2ChQjYt2D0%2B0382ZT%2F1Elx4wifT6NjFqo0GyZVtRwwntXJfbsqAfsGyTqGBC6LfTddx2CuM3yl0RihXomnPefAY1izpLwvkuOGBIxdzv8htdMm8oactVP0PkDnVJ%2BZT6l4KPvxAWx%2BRbf9v1PIyMmnoMLidssYGOqUBRSHi9GWKvpFuwO4mhbhCAKlh2yRndyqmXLd2VP1XWWvmbM2I2wMjWBjliWxl5Jptex8ha0bKbmd2BVX%2FFFL1syXlyd1tPI7Z4Wd86zOWzW7li1yHpuDP0wg%2FOmvbCRgFHIAsdO%2BHwk%2BXKxJn7bgtz4PuusAQyVDABSi9m%2FoL2HDht7xJfQqEGaebGzjm9zXi0SS04RBUYw1RjljkFP%2FUuElmZxQn&X-Amz-Signature=f72c3255b3ce5c9a88cce1aec66bec9fb6cf3179051a6e77217c1dce6c61c214&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

