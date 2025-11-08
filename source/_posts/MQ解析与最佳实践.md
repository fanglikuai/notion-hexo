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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TZLKEHNP%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIEqeLDzorVmaXEyIWhDbWPcnRUDl9zlAXQ5hsky9rqi6AiAUPvfrjrSOAj%2FJ7KctaOkFIw0Fo0gaH6oUM2RKH%2Fa7XSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXVkGRrT5R%2Bygr4NUKtwDO%2BoRoqv7TcV8R2Y8f4LZI6yOMdjpVkPJ4Lp3atREszBc01ee4wX9%2B5oKC1VGqVSdmufN0b2TaGYCveWFizjkztTsVftP%2BYbLKWxfnypAtdAvbcOl5934e5x%2FzxshSaK03NxyCVRjz4EsY8S7Ve7uVUKCU3yU7X26RthFkkMY4FWJRQ7ST%2FEaOSpYBUG6S1RhxezB9Ro2R64PNdBkFlqy0ZrAn6t1vZ01jWtPKpekh6Koxeq8cN8jOW%2B8gdXjsZZJhZzoAUnvqU1uSVo5erW1Ts7aHp9X8wffHcmLDUm5yb2xZEImbt5OMh1%2FSaNnR3rk5M8MMhHZrVzgC%2Fl6DrbrcTacGOToJ1WYzmW12lVCz2wJiWYSRu99Zb%2BkJQNxIBV3ug%2BsU8b0NIPWdHTdGK%2Bxk2MIOEnCm%2Bj3ytedk69zJV0lmkZ2aPGv4iejrQx8cRxvXiwVUtanOpwXyn%2B6erRhk82wSabIKmF2nsRpjxiFpGC2d5He3Bk9aH0c%2Fya6vuC%2BSDctwjZKc26zCbaFvUnxgmjXmrvl%2BdVolR5kfDx2aYC%2Fdl7DqgWRyPhIJFm3DGPgCi4XWDd0uPBGefTz90sc4Ls65BJC2BXGBbbLa0QP3MiKL08jI3c6UDkkNOsw7d%2B9yAY6pgHtG%2BKwzthI3pd8i9oBm3mCIosyUqbhXc1DXm0cpbZhC18B3AJ73AF8mqTVk6HjsGpHeAug4nb5W2VzbiIAvvT7vysMYPkExBvByhsSmJDltKT3X5TLkoHVMQUF8tOK8ydtdU%2FSQvRcgo2zY0CC93KY4SBan%2Fk4CHx6fxBNJcLQML9FbhKT581iM2u7vxFklb84cCNncD95Li%2BBXJaC7UulFi4ixuL8&X-Amz-Signature=e322a2599d408d148b2877a599e7dc9532e19a2aa20abbe5068e11c149c29645&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

