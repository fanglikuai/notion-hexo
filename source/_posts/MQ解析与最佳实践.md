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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSOLUKZX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T120048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9xpGBJwolkcupLCxomIPfb%2FR1tyB6W0rKxjN%2FfxdbBwIgaVHNTI5fi6WodAbmJydfCUh%2F0h1DGd%2Bp%2BO7yS5TGO%2FYq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDAUexFlmW5CfJ7x48SrcA09P7sSXOXz8tZ%2FfjWYG63%2Bdnm8lpWTIjk%2Bo0P3DegjDpSmJ%2FohIinnWHtnsJaoBRV3WMKoZodGYwcKk5g6M5xk%2B4XscH7VNvM9VYgSTlvfkzDEINdrpYzpFv1QhvZ0GdH4sRSqtDlDNj7tmeGfGJxIhVyuBeTVoSTas3yCSM2J0FJJCNOhliriCtd7PZgb1%2FSHdI0nfF%2BGbFtezY91dk8SWy2ng76qeqDeusLmwM3QaIrq%2Bkaql88vZc2JNMitZMs5%2BS5YqQ75EehH%2BztjzXjCp5rXpjTO0PZV5P7EqBkfNl2LfVmpWZ7YV3DdVPi7EJYG5K9ST1NkqdYPjjgjyfHtQxPLuEUwlQ2XcZnrzSXVpJFO8UhwFIkZBXslVm7vy7rVkdgZG5%2FzhbMGRqzlSehc3QQxvv7ADa2AxolRgcHghqWxvo9fw15VMLmkOJNN%2ByoAXaCb1l393LOU1Iac4yH5V9c%2B1%2B4gnCyiKxYmONGsrDwB7ze5SJTaAFgjZgPqQ0Tj8YsIvyUs43%2FFP75lfzw6g1K5hjFgxXW%2FMnR%2FyMTyLz9reZCy0c9CUeP8Iq1BD6AeihKxjCZ6v4VVq8%2BAMn62Lv%2BT%2B9yF6vSAVTkGLSlQ5lEMR%2B%2FGVRiyPmR8GMO3Ps8cGOqUBRdV%2F%2F2mwX4OFJrAnORxLCkAz8wX0urpBlXEmQhkOOA%2FdBqZVtPflH4khxV%2FYe89rJeIQiQesw6%2FtFFdPi2NMZWUUUQiksdamvaBUNtxMIRgZcWgSj6FRES7eoyGb1AE74sSz8S2Vls4KNhydvWpa6Tcr7qDpsOn0g9Ix%2BPQeZunuzdTBMnjg%2F1Yb5CJBBjDzxveOiQvyeLuOMu4JnojmqtsnbXlD&X-Amz-Signature=350517a6b76d26154d907cd4e7b475e8bc358b788ae875948d40c73c3403c1fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

