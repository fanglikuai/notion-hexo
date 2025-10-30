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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UYQRJLH%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQD3QIhCuuXPidDDXI6OWPA%2BfaoQ9D18PruPJRukvESVoQIgOoXrxEZ8S6vQvRqWP1Jn1N6boGKP0m4SvbTYObadmbUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJpRo0L%2B5SCzESGqyrcAxDXSOdjvSc1s7yruYKS5Q11d0E6Ubi7Aj0qGabJ9D2EpvM0pWzwcVlTx18O1mD4Q1PE66imADVG52tsB%2B4JqLOl5xt1vihTYPll2CfZQIKY%2BQcz6L4ScNrCLfL2F%2Fbq0TdKRBdZQPoeJ0GCoztyGoK07oXo5e4B9OBfaBwPM93EaJEATeSKBMrd29Q7mY2UxDLr8Br%2FLq5nS1e%2FARCasdJ1J7tNPpJhgFkmYS6TeKAlsFj%2BeXI9XcjSDlEzRAjBua1kTaKmS%2BvzZjNgR0Y6%2B4zHjcidHVxfwDv81clkx5Wzzlk%2F2D7X%2FLP7kzVaXo%2B1pLxRE6tKvkL1WX30x%2B9Rl0HSzGWndiAYT%2F7%2BfKmpbrd4xUzyIIbx2Lnyi1znInZZEyIcQ6x8L7fsqLIu3q4jpsaRsiODnb63pFOmtF3Lzcs9wuECvIQ5095sY1UxpcOaZBx%2Fj1GD4s68NUaRkoyBxLsRHf5IrPSUv%2B4Q9ph%2F467ykNwE9gEUbZD7GCA9l3sTJufWvK34HKFstmEK0BYx4c5vYbGxf%2FcEjwFCdifY9GYeWgsukxbCPY7%2BZsAGqJs%2BvoLVlvUGYUW0xsrjZWkF3lWd%2FVhrDOJ%2FTPY8J7YQaqBO6R5pq1FJ3MdmtMAhMN%2BcjsgGOqUBld0%2BwNLw17Tc1BYFrnOV9BRrUkxuxK%2FQQDAVY2MAjqRRhOyRehO7rsGuuAPCMGdiwn8kxc46ZAqOt3z1w97mWRFX4P%2BmgSslrz6y0%2F97U%2BxOvrXIzyeEiZMuEYNvys760mFhJoiU6rL7RgFLDAYUlOWyuTE6bOxJs8WEE6w6cEzVtVlZ1AshBgNvobHjnvw%2FnYY7gQy1w793Fo4jzZWMN%2BJMLyP7&X-Amz-Signature=04a992b24614a3d8f149ac6df1e735d57840ffeea1c06708c62afb0dbb65b81f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

