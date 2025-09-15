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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666LBA3QYN%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T183042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJGMEQCIHOeh7XpkTxnoOGyKdf%2FfXgkFYK%2FdNrXl3oI31eYrXsfAiA0QrgXJRqpTBQE5J4HROSNj9UOGyieUloRoVI09KNyLCr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMrkFAoH40cv%2Bpy9VTKtwDX%2F0ffF%2FA79zZ75pSe7hvCOH6iGXD4Ra89RjrjXC%2FEQmzcsEjAVRMNuEa4F9EUkCNnMSVJFAx9S42Yk1FMKmjEfKnaab0139Ak%2FQR%2F5BdIW822fUBTYfoDTxqs77RemT7MElJpKw%2BStRrBRDozjEJ97guZxk5hO%2F3uohiNjPVbgqhwkCIJt7mYGIizOcyFPQSAZPX%2FP8eZX40NZYr0C09pRQ8V90SKUjvWJwtbWgtwt9GP2GZbwwGGPf0lf6jaDxbb1jO8xAP3On%2BQjWGPL3gLP7mdfWgaRY0ljDyAFQ%2FRTEEd2yXqO7E7TTdaN5cFSfIUpIY228JfBKdnjRkw0MeTPyZ3jiw7A6XOLoCPFoaDHmiuLrLNyYaoH4KfPtoDd0L7U9KDly%2F8abjpB%2BaJRSXhnYEz%2By%2FklCJi3cbCsxJ8fIV5aSGWi5P8kQv0MZ7hITYWGd1%2B3qMR%2BOcX%2Fb6XARBGGpcC4b%2BFT7h%2Ftf6ggXPJ%2F9OfMLYksQkwbWbdo8BCnvKJCBQJ1cpr6OabyUx2yElNFDvhPdj%2FI1OjKZUP4cRepZkWansO5ORK5gfB83CmK9Qtbzz5MJWDmbQXnOEs4usTTtVRtE5M76Cp1%2B523kpOE7Qw4q18yMHsoA7SEIwr4qhxgY6pgFh1oROvIznyl5360xBdwiC00mzwqHMwsrFZ4s2WMrgkl5XItkyuVqxE82WVzcE1Ob21PKpZyZ%2FrADQzUtxpjyYMQWPiMjtDV%2FpVm%2FA%2BJJfev9o7uwzcO6yxU23sc%2F7lkS91esMZwuupqrtJ8dCA9Ww6AM4BP0Z9mkss4ffjCZjF4wrERrhd7H3ozzPGRnE7Os5uqyFZMBzjKQVs8tcOCfqmqxuT9U%2B&X-Amz-Signature=57be23521b0758b4d4d69ee040bad5834252e12cee143a2b9ad5b6454d187ad9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

