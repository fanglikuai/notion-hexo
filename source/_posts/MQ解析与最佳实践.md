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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WOVZTL4%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG9qDtU%2B51wYnHSjkceno5dji8IGU4dFax2VFPwbu1hCAiA0Q2TJQhjMZJm0bZjn00YmKM1ac8H2mYwgD6urRtDj8iqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMezduoEizk7MgIjYKtwD2ArEVwMpX8KY%2FK6nKeETy2NbYsNcfvBW3gQF%2FeJ7kghjtgoHPbEQPhN4cmf%2F413edwQpcwmeXTo5ILeZGR%2BhUkHEXFpWBGaK1wxwm3a8CmicrWx%2BfUT75CHCo3dYQiP1GhRxkQ0h5QHqqn%2Fp6v76Nw1bEWnCYD1FTcS8KiHb1JiIYb71JEKsWyRE%2Bxllu92I7GPoVKyDGZ0Ftht1mN12Ng97bFHTKrzFJr%2FtnPKbUiKfx6MMO9fbioNT9ZsosepycG92%2BMBsmTpw70fSreie1NFHiN9h5PjqJ4YNKLfnpP5DWuu04USkdNEYvBfPb1PDKDAbPMpRM9uj2f3oi1lS0IGIvgv7eRHX4uYLUe7iY5bMCB%2FAIopZVecma%2FCnoWJpbhdwII0xYNTekc7Gkb3aNIICKF9mxdbse261qIb4japHiTDU%2BBE7yD%2FdgCzQbl2vEbmKYU9ShSDQ4kQhuRLzC%2F2kGCvNw2scFsx%2FGlmzTWvelynWyIw6Hv0EOdBKpfJ3B%2BjPp6eujm53N190gq8Mhh04NZxtRqs4o%2B5Ru9fKT3tBOA64XTcrA%2BatDJ8iWSPxjy0bqY%2FHX7PiXZrSNWz%2F7MvhNtlv7ebg1ZGMqs0nlqdZHyeWPnKJiuL3m4UwoaSyyAY6pgGztogYlANf3sAWXviBmo5OyKnOe8bv5v4nTnjYkYAaPQQqfGgbKNAt%2BDiwOOwIUznpHIIweUiYsQfMqHFyBjd7QdLGCPvOvoJ2G1qJIVeERKFOd0D0p%2FKCDC3kFsMk%2Ff7cwoRnM4wlrlibQAJB%2F%2F4RQZw4gV3cCmTPAAC5vTJvjT7z9GxkUEYcE2tJUjiJZ07EFhE8eY0Q8BHeJVgRcxY7%2Fe2IqLHB&X-Amz-Signature=a90c9366ba3b23740b54e6f78155b678acaf890356009d4180f529cec403a2cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

