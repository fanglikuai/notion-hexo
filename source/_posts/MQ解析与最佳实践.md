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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A3ETUC5%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID5kekUBd97qhKo%2BoGj7l4JKrzBgmPuQbEgQjRnuQ8MGAiA90C2Z1D8i4MBQOb%2BuKOC6snN24BKV3OsMo7f3qtp9Zyr%2FAwgbEAAaDDYzNzQyMzE4MzgwNSIMaaijw2e19L2hb172KtwDUbGqGZDtWUx%2FqMdzGBrK2lqgwMSymlJlOQ0M9xZTQLrLds177MHUAGD7CrMNXRvrsrMbD3MATGWXMzWQNr8K1I9NOdzimIaqSLKkzX8t1XSoS4U%2BOso6%2BZEhZ7BuKZybVWfz8m8gRao9HdtC6ztBYWyL%2Ffugul3GK9TpIWaAKJAWdYVkoUrykYiF7%2BTuEs0Lu8kIz1X%2F85ZKwP5Jd3OJqVhIMovb6r965E9UupSrTgPPyp8EL9Doo8KafgWu1n2iDzn9ZFkP3fYWw3AtFKwJbDMd3QI0Wgn9lP5Sr0VZhbkC5i3RM0eCWh6WTfliwKbZ2SRTwBUkNHpCVhka6f5vdqiVVuR7gL%2ButNmM9JMtvyLJ19fpKI12cmeBX9A2xQh300BT101jGCXarGIggkUsI%2FMKh2otd%2BRYwKxCIljzae3Y1K5NZkHMr5AcULGdV%2FonwOtZCf4YMSVKf4QICue%2B4Elll%2FKJ8tHieXM1P2FopZ7rDUagLmyUSY7wfZbdSLz9zJyiMkN9%2FqoZz6Ef6hBkI0tAqDJ0JDGhvn1InnGwUeKETvLsfYRNIjFliOrJo1i1oYerBoYqQAKIXPa%2F9hU894YxxnZcQlWhDivQax%2FwAuSDKaFYVu3zXQb0cWQw3OnAxgY6pgFcJN2%2FOPK7zd6r9bSiDW%2F0nRiGWzhKPxq%2FePU1nbber%2FQ5372Wg9fEVdoWK%2BjxQa9VzQ%2Ffv5Z9bN%2BHcBlenMGjpGdrUKh1Shsszit4P0Lhh3Nb1o7AK%2BDAy3MMwknb71b6x2%2FtK7GEjnHZWKnozRD6sioOCtvRfJFCGgZBo99mvzBjnuOE1Ocr4FpNGScZ45TGyfWrT8imepYgRJu%2FrSSWGEZ9qmZn&X-Amz-Signature=44becc5fa264b68c0a5ae9075fe3486e4f47645df7d9c0946101dd0ad0f452a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

