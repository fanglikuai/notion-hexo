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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SUGYHPMT%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIBi3XFLOiZhkv8ziLbyC2hC0mTFWQQuKlkxpaCb9SI2UAiEA8zIvKRCFYV7hBQulrq9YIsZrHFs46dwF4WJHWTEFwh8qiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMK%2Fa9oHQ%2FChpeaqlircA8XOWG7My4lgON4%2FtYGh0sQnQAsAZvX1ym3LlDkEAEWORadfigFDHm4bJrY7FEmXnjMGGdphLX9rh3wK1zKAcBYqNegCPVhESZ9C%2FaoVqCzGtrNb4ZHPX%2F4jx8ccg6L7ptF7mnVCBtS%2BgFwRYtbw%2BNBGenyAmbAYEX3qNdWrJymAQFBErV%2Fx3DS20yFGIkH3HJfgSMtCDQJuT8gV11jHXEQp9zmCRHmWH9Bkn8H72ItPCkVQOCvon1iBj%2Fl0w29jVq4hj7oUoARE5Gpuipt1%2FGUpPOXMDmeEUU5R43cZEz29XJTvIkL6DlysHRtGKpFO0oLnCTulGv6dWpq2HEn%2BxJNEKN7VJkM9xZeE8hjalpfzm3wP3H15LU7ClJDDDgvxKKzusSI1LqgorFaIQzJZd9zaczakbniYpEKjyitoiEPiOVvaBw81TjU5TQMh0JqR0F7ESy0FcI8Q09vlpzCc8NT%2BEIjU%2FLVta81FIxfV7yMM10licr5ISny5%2Ba1elJU4i6Fdk2zOgEP75NadbIDrNSo7FKKNrCeWrAesiToFnQhiOlKXP4NCmx01lcsFJXkNjsbq24iK5VgOjGU1YSwmoVrcYJvXduDbytg2cYMa4NL%2FPvaX376Nk9%2BBD4f9MKXZ2McGOqUB0qQ6Z3nEfhnSRlCmWNgvGBjJq%2FCOOUFrHKpRkBJZFlIL1yfAPeF%2FC4N%2BpsHkPuEZagY7N0snF2fIyYvwz8PuPXprup433N2Lejhz7mllAaH5p2tiRV9nt%2BtYtIqYnOIbZmRK07HNYRvKXhBz3C4VIbnZQViopzV9qLfTyXZjc0XkTnh0oNBOeoeX3pZ8vkccFxHhTEn%2FSAyhvac31pUH04mBtr%2Br&X-Amz-Signature=ad5de0e1d8e5088e8f65347ca5e527a47a6a701139410de38ca4770b9c9fd70e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

