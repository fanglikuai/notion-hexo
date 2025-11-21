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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3FWRJPS%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJIMEYCIQDTCmJIovHzgyUnVEFbmbWvYhLU3bEm08I9CA4Y42TeJwIhALyXIXMsGX5fJbWMhmhxBFYTwuWMAmiKA5CbfB26ivRwKv8DCBQQABoMNjM3NDIzMTgzODA1IgzO2N72CxtojCbP4dQq3AOHw7IDmDksr8%2FPfVVmp383hmdxMPK80DTT4RRRTI6jbfZSeKcCoWIOZsI7ZbKJI2tAZw%2FsULFNvg%2Fi9imkY1BS5XXCQT7J1c%2BBIYbozgLfJegu5OJ3Cpxw3O8P046C0dMtcyaV0k8XZOw07Wo%2F0Oq1lI6DgxVS2DIy6ALDywPG49uUqSKCECnScvc%2BEzJqPqJ7szQol9hY1OqLTGPNIXtdQUUEgAU%2FWzQ%2BhoLMrRjEw1YzfS0TqS9LPPKQub01oKO4%2Bb5ioEv6QPkjh02%2BUpCaEJuNb1ZjuO42InE1yns04V4xlR88bX35g6fZqAVUSsLFcHrjqDVlptXEy1NszPDLgHEZy8rMio%2B7Wygixq07Y5JvZsFDjRR%2FyWxMer4xNQBXtCjGDnWVvLK6WgkvEUtOkNHSLMNVXvZeGybV2K%2B2bB32R29yeucrX4LgH6ArjSb710EcjV8p0YqBHbOmkYK7aHdQWJ9hnv%2F0K5hO5M2OMR0QIvuXAhrlvgDRf8ktHHKOW8u4WMXoaLhd4Kr1EVeZTrgxPP4BjXOr2Wh%2FeYAOvIEFcbjdFHg2XwGl1zP71Gw%2FD8Q2EaC0G1lC%2F7yO7Od5QZv%2BDnfpff%2FhnWsznfhTVtLyGO%2BWuJFnCX2gfjCr6YLJBjqkATDbY%2BcVN20ywosSQqglGamwnA0sDpd6xLG6r0vXULn1XDyUxQisvaI1PsmIR3K5hXXA1jaFQZz0Yfq651gngl7S4JJ5oj1I5CRvTQMa0b9tyuAkFnXQHAY%2FlfmXB%2BxotpssCtsLvDDi9GbQ1xyJxnDloy6ZjA9FuvGTUoGOg7zoZd5pJjR7bQD0RzLHNHaNh438G8%2BRqMwC%2BXf9RZueAUrfu6JP&X-Amz-Signature=03cf2cb96cf0febd5a523113f65b2cd42e54e2d8a561189565eb660e1d80015b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

