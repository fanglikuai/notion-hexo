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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKXQBNHH%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD6OTzKPCmXnMFxmv%2BgZtHpUgzzvVB2%2BX%2B3u%2BxQzsn5NwIhALTloXeoZQ2%2FboE6yCRuXO7b5VxvEqwr8SD3PgbXA9fhKv8DCGgQABoMNjM3NDIzMTgzODA1Igxxv7tYEeWknM1nkdkq3APHXrsemT4AUDCozNl46egCLS39zOLN5soS36dNkqpq2vfDDl2iam6EBZHXmmu4d2fSwDYTbWGnz9kITCOOkEBcHjL50Xsv8RmxvkQR1yT9Fygf%2Bj8PDRBG%2BqaJaUZuLAewHvCHb83T43oQEGhHcFUiCqh9n6xDIDcE1duWHsHDcL425MD6rriYwSydvBbXMRcVX9PvIEnOX691LsRTNxz07uK68CJQt5L3R5gN84gCEekrIe1gVt8wYvUk%2BZkCIQsAA9JmNrCx7%2FfaXJff4LURDYVyTktY%2BMGcIjaoAsK%2BC2XwrJ1wOF1V95z4vAZ%2BnA%2BLAC7ppfwWVLqfEAWEESrwHLZb0xwuLa42hhn9uzpIttCxVG71%2BWJZ3GbKNfjzYELleVEQa84dsLsc1KGlbS4tytbSaeY0zou9RrVS5zdLqGOqhYUSnnVCkVECnuUWlYQR3ygmNhWHvWaSy4UvedTeACgyK1OA5g1H43KD4I1su6jpQTZcl8QRbi2agAHR8KpKZsxS0IgBS2DZqKZfAH4rhPVFUB%2FJkmBq%2BHHML3%2B7udDznWUxOsRR%2FeEhXn9ZN6V2r%2FsfT5rWCeGEIg744ZxcVnwUxhdNL5%2BBkDJLevHPZNKbxFI2xc1XTxMkEzCkprvHBjqkAYBOCKDIO8JFyqvvpQsGE9qSM%2FBaOvBXNaWIi3IGUM3aP1KWNjxHwQ4gQ%2FEIp76QY2i%2FEF7VGE5nnAbidQHE0uBGW3A2l8xKe3REwcXm56szDsDm630xVQolGJh%2Fe7L2FJs00N1MXjI9upoX2ZhwnnxhQSL9YB%2BNhu%2FS8I%2FN4IncqTcH%2BynhnZsFbzdX9HEkoyKxTy4i1Xg9%2FAyMVgloA4Wcd6wO&X-Amz-Signature=09cb63bd4dce997c39aa4760553ea63e5d0135f38f90d95fa3834eac52e7c724&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

