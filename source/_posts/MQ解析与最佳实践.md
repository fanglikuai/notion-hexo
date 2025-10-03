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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNXAAFEP%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFNsrUSD8JpPY%2BClehlxQ6QD4i7KH3oA%2Bes%2F44ABwEyyAiA6SCSzr0FpbXpvFyOo%2BLY0MCp7viOaAoMyrqImBqb6lir%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIMuwnqpzGAgq%2FOjTbzKtwDAmVghHAAD2Pf2Y6t2OIV2gdDO0jIqc5y6glgi0bwgY%2FY5SzSe3hGY7dGeCCltnfbLiDoVJLB%2F2WJ5mMyB4%2BBYNVVWesQTJ09sNNLr1kaxODW0j5GAJrzmljdHFqAl11eQ4CJlhmfCXKgLhKdaOurIAGm9spJgM7lV7RsKvgj4sQTCI1k6%2BWQeJwNx9e6Hmi5%2FY5nB8P6K1zUkKE%2BadncVEoMZg85zfBmTGXZhkorhlJN2Mj9UmjqBS2d6kZoC%2BpSfzxyDn8RgyIwhoTlDw8dyH54nF5hHKIMh0Zh5gABfZ99WxP3h2LwS8brQWkKUTwg%2Bc2YRQ0nVaN7T6B6d%2BZwEpEJIcQ2rTZepiYoyMy1bDqOzNDnYVq4i5955%2BWLk6tCBkJYhOQ9pStCBfxdtZ%2BHQ6r2lLrTnG%2BVlW0dmG%2BBybGtWYD4%2FkkFlDuYyTfz3jbjlAWHlU6C3DimMbBkYLkseSPI0Rt5v3p9etH4iVWPTRUxPWlsHgWBAIcTMhmLSBLsX9iEY7kJP5lzROXaleOQA9ZRyDfKeXY6lY37vtlyWy8%2Frnw%2FWB5Hyj%2BHthiSp%2Fi55Pufs4oSEF3X6UNXHAEKDoIl2OiqDiiMNn%2FiFmK0hoeOnbJbnu0sKuxf3tYw%2BpeAxwY6pgEDTMSHWpxLlT0UyCBmXUhoToFCyulMZ3rr3lH%2FOTw9pf8snoI6R092Pd%2BZHkapO2zHCsBI7HkcYAQPwlAgSx739EW%2FjT8bxQU3fEABLC%2BSSu%2BduaGR1TmveEcTBIwZcO%2BuiivzFhQsG%2F2oUpgbMQ%2B6YO4W2EV%2BF5Ugx0fuwsbK1g42nV1vHs1OMXkclwCfZzmJn8wkevJwIhjLFwyNgPA35ZA7iyxb&X-Amz-Signature=6add2d33e611e82487c1112b9a6779faad484001055359a12bf9c195e2128693&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

