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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SD2PXNBI%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC%2BtVpCqAvIl6XSv5DfSFyxZCkapfXHfC20%2BTZu0HNlDAiEA8YPaLANk2YiUl27ciqGF%2FqO8zKq60wY%2FpTD4X6rbpOIqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE8KPXfigN75kCT4YCrcA1HNiE4EzPOE8HRikVzU3YPpk3%2BPl7CQh1LgLLSHNJRPxaY47aNzgYEkUNqW1N9GN4n7mKHBri37bOlnBrToG3tf7CtkoKYWFp6z%2B5bO1sIkyRHWkzjEJ6WQG4tTgVAE%2Fwz0uHBaD2R%2Fh0Ht91PyLVVixNybgweHBjmuJNvDsdO7xgQZja9TPfvyZ5rvu4bb%2FE3F%2FJFFprzyVoxzG7Cb5Z0DXWwj5x4UgMwkAn1Ey2s%2FJVbTCjoY0Y2wWW0PT%2BsS9%2BfhemnYnbQ0AZZKjBZW%2B%2BYiFvSk7%2BSyFJRHAZvgNAKaDWi2nrxOxBhMJ%2Bx6D92cd51uCDTAaBSbxhJbbTqRGAaX6RyvBmuXg6Q6Po5wIknmdVfV1oQQTqOgQ94WP0xpADqqQIPc6L2xDTIQSb7es9LVNnO8pdQZ0XJyVtjHymye%2FvfmvM7Fv0pSVL14XugY6XLmpvirNKt%2BZkTJNrpkobe72bGmHhl%2BjwqB39m0fXO3jXXVIZDROnG%2FJ9tOwQxghraQY1oR0dc46%2F6gjgdtuv822F9o2CnAi%2BJHMc2Kdeo2GEePlK3hrP%2F4XYPgBFZQ%2Fnjn08bw9TnDOnhkb3pvTvl6hnVeBA8Z%2F6YnPN9gD3%2B3ysupCg8s7kDaiht6MMiKxMcGOqUBm6F5aLBV4LBFYJ%2FH1YAMC0ZwfDg%2BAwBOPYaxfwcKpMYEIxLUfCcZLVFqmTz5YNGWKHx6q4C%2FDJj20qTEBuRPZUl9s1KokCXiIPxmAv3rGDTtLD%2BxtocWPt5iEB%2FeVw6elhVkDbTRN04ZRZ02sICOntzgFCWJRwD9lmIYTbhY4PL8cRktqhAX%2BaiQZ0PxfvhjzIQkfs%2FSUl%2B0TTxl5500PWLKNjAO&X-Amz-Signature=b2c463c702cc430fd5ff861def8a0d99d899016d99c7d3b6ff008ba0a573a1b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

