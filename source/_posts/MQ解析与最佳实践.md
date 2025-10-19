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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HL2VBTD%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIGFM7XeVEc2fO1f8pgW9%2F1hoJPq4bnnRsfWNQH9S%2BGD1AiBsOgRhoswlCBh%2FRWefq2A5ZXryEYgmI5BB3C0vtwqigiqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMD3A%2B2%2Ft8UPscYe7DKtwD%2Fynyc%2F7jw%2BRmrs3rb%2FJ8NwQ9ym%2F2Uuedw5UjyhIVmvjpx9e7%2B8vy7VG76s%2FW5eNwH4KzmmMcSElgd7n5t680tPkYgOuP4zrO74uy8F0CzXKkhsdycJJV%2FyK1JM7EeoQFISu2oWG0a1pQ32%2Fyi8CkpL6Ej%2B78nEJaFIz%2FMnxoqoKju7kZ7OklabRQvUERzpm9zKMmqABfjqiiqSmpw7ylzcxjlCES1KsRcjPkPiHO%2FGqIw121d0rbMgTbfRjdRLi1brcVaI4u0BQX0rT6EpCJjT7wIUWQ4qZTKabVUg%2BiM7XoAEzN1U%2FmUTmNOTVK0y3BtTHhoDa6Od0o%2BM4HPYdALgOhLakeh6evSsWwSlNlYRZrc%2Ba6qW7%2BEWJoq633wEbAvx%2Blm7fomhBnJwYONoYzOR%2BdmErEq%2BxvZ5yhRLGLZceGSWLo0TZR2%2BJie3Ba%2BO3FXQOhJTIpgBWD9IqKckjUcC5iRHKEjvFfB90ui%2FcvUtZJZugg9rEXwAPVTF5L66dbLkb6L70naOMsvYXWi0Q2nD5J9QigeSga3dY4852sarN0Jqg%2FvTaYqXa2wBbOEg6rnS%2BRjJ0qX%2FoEV4uJLvDMe09UCTlI7UPeS%2FyPz%2BHwXQ2ntiiggcfo%2BwAFcFgwsOnQxwY6pgFzyYg5eeqR12H9YJxp4H7FVW%2FFSYEqfL0xxRpMFC4x%2B5n4edqkY9fK3Tr75fXbhDnVu%2B%2BC%2BQPbd%2FQtYeJYOauKQKn3OfYuI0OIbgTwB3j1kCbrPqzBIHVYcyrk%2BDysp1CdzR%2B0B3jXkFzw%2B1mS9CjsFuYwy%2FxQFDmFeS1%2B%2BW8ujvHaruce3F6kYktvd%2FQgvlRxdAcnTZJ2mAD%2FjKK%2F4Pe6mbqQmevB&X-Amz-Signature=87f23b74bebba5c73dc6fd6aabda0e202286193a271ba6671c116a028b717f47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

