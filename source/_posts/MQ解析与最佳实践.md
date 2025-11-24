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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZO2CUOA4%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHJu5cj5OBVlFabyfvKtaoOK9w7qlxgXQc7ZaVMq2a4gIgVN%2BKJU%2FTj%2FqDfY6LQeiZO9aB%2FIAQUabembcukuYfjHgq%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDKC99MSsjZQGk4ODHCrcA5DYTTsrfGjjRzN4UzogvYEa43Nko0C4H%2FcqaXjGdu5J%2FNo9Ex1aUoyzK%2FWM2kLNYVYV0%2FuFwRu9Z2m5dXr397X%2BMpxf8w7%2Br8nR%2BVbjOgWaBcp6%2FsufSAD%2FlcMceTHmhPfJ4sRGfwUVkqEbYPlJv%2FxeSfrxUKDwbmr3DqIiUGPT%2B0WiMJnHZzB4OyVB7%2FVoHEVdMysiXoUVBbWE0k3E%2Bj%2FZUwb0oGi2ztBcKilYd4Y7vZ4Hi%2BvJ7VkWErnGlbHRtIUyS1Bty4IwMFnPbDJjyxU%2FiVd4WSdASK8jTIVaACWJYBdPF%2FjMcDookOhQ%2FFzy0Ks641bL4Ih4O3te17zZvlzO7qFrlBBZHfb8wun3MJjTXMOV0cC0M9%2FBN%2B%2BSHWza3LzbTqNjuq2BQEU%2FoSFDHXfB%2FGkI8WV6Fz0PVOq%2B7YsnemcRm2I51%2Bm0%2B2o7wqSB4lXcmJvrS8GT7iGZ5O633ERm8hvuaJGbiMTq0qEIQL5t2mMVYfuW0ypYl4HabH7i06uAmxsEk1aQ%2BqeiPbytYY2P4YQcvqhM6QB%2BT8cqc7%2FO087TSVVpsPuYQt5H850onfrGBmmLMcPZ5YHmFJRXKJJST3iXVvuPhHMK8H2Mfxpj0MYSlmgfqDQNoK%2FLMIeGkckGOqUBNIyoapo8XJaCmpZy6FPulxg8y%2BZb8goS4oUwqbs%2BwEPl5Xltv1nHTVq7hKRXUqhESgtk3OorL7HCfiqXLt%2BJLjADyMExK5QBerIBJ5%2FDmuzg3uQVUG%2BBCEZ0vP8pFvNZEIKHLckCd%2F4mEi6mgw8YUhgdakIvSdjnvw2laQsWh5rqEJrU9Ep6auLANjaCxdHeGatk%2BacpX0z6WD%2FnUCuIJaACseMQ&X-Amz-Signature=0e90036f17d20fe0e7ac605fea971097c3d2b681c9356c8fca325e2cd8e9db54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

