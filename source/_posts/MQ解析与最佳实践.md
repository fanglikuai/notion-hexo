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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCLC3IIY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCYbF3O8zicU8kicQ612PjeY1JGwxURSXT%2B%2B%2Bbphd8kagIgDhfhU%2FmFjgRyr44%2FoZ%2BzSqNaF5dMmI5ykk1SdIlUptUqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNDmuLROCAS6YFJsBSrcA%2FvSdw1GmcdyzK2kX0DJ9bi%2FMl6%2B%2B7yTZVudDFrFPfFy8bMXnDncFBpqFuXZKKtri5bJRqZ0Yqq8WrMb2c9hAldRbuiRkc62ykfpE4CBRJQfZt%2FEo0bTW0IthH27RErxLjJqVxNHvjWxKwRnJ0Rd3EvtXg5vvd51F3zLsP5%2F%2Fm7LmHxEqT3IM18DBPdQEekOPx6JJwOICRG9HK1RwX5qvS8DJoUM7dwkpeI0dTqAGIJRX3N7VVM18my4tvIEQxi2YwGJ5lWM%2BCTSBDEE1qSr20d8ZO52BwWCVDKtuJm7p1W4RfEofkXpQ%2F0Hn8wsJJkKOKno%2BqqtOjkjkk%2Bqyiou1rTXfADwgxD2xnCKDtkvmPFMqPLVw2Wql57uF2OT5W%2FNQ6b9l0XQGboAQKWC%2B0SuGYNUKXJQBCbBYQfChZXwP7oEfRYrRFbh6vr%2FglHYb74%2FvItC5CAahCKN9PguyY9KWW01YrR%2Bw7Irf2khHIxICwvuUhbnHGU5me2%2BaDp0klTrOig5VD4hdVgv%2FICHzmu1HrzYSb05h1M9ACbdpd3fiy0uwuBq7Wver%2FS4PqNGyDnJNHAaHvhMQnko%2Fa9OhnC8m06xxLcpD2a%2BpkgnR4NDYey0faVlBUJrcnMan2sKMM2koMkGOqUBxGy1UwcrVNDAHmZ2I6mxzO9CrfoIJxnTCRYBgD751DRKaubHRTymFgk2vSr%2BzHammKPSj2e9hXJsBOpgkh9tYlbbpZwg4PRGbFztET6B86z9FJ2Plyu0I5Y4hfgK2H%2BXPSUYZE4gl3yYJHN%2F5S93tX7r%2FTR0Doz5zjxgbmVNsB1kUU6BIiid77aV4p2LOjLY8%2FZTA2BvAQyRQpi78GwsC39fH7lS&X-Amz-Signature=178709b29548af0e751ebc2fe97d46ae8c6d2364353626f0e9a71cee8d5f38ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

