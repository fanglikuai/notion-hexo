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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMA3SS3L%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T090134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF7V80cxeTePp3HhOi4PkBrQe9TN3QPex6P1MDHTLOFcAiEAh%2Bn4IJe8dyQQY9kxwzruoqc9Q4yfFh14jLChX0sFvFsq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDJ25Hu6TjU5%2FaYX3KCrcA5i12u%2Bm7XqF6PKvpyMG244I6RJistR7huAb2G6QDdEYawCtboi%2BQ6mf2I9RSvNTR3axKJa2vSR6FLNqCbYQb7tSfD2C5cybuOq2LDzBnnKmmXH1qRfUXEgimLXyKlho14ZulfHOg3dc4uDcZhYGJqrCRbTD9XATeRmfRCdGSzFIuCj7GLmKQZNmsXjlmz%2FZeXIH%2B4DOkaZwVPiBUanWDuiRPe%2F4a6KTfX%2F4CXrpoAGHq%2FueaMURNfcRIqTiok5Z%2FpCaIg5cgg2pBqmPrPezmdj%2Fm7mQt36f7NAiSZskEIa6ZO%2Fsjl8G2ego2F8fz1YYPFCcIt%2FZ0JmXd4tLNxvHO6KmNqDGzuuoHHSZkX21DzU4zUt8TwIHpGuea4Gv6jPm4afshdaE%2FFm9Wp0GGy8CrFJ11vGDVDvHAC8Bq9M8fewVwixab6DmX%2FaR2mMS1Np0Y2%2F9xlfS%2FD6Xz9%2FIcdLBoqxlX7lqJ%2FA%2BwM5Qe7HoZql2oNR5b%2BfTm0ai6S8atKnO5vzKBB00Y4FAgxAkanRIEiq6Cf8U1JiSnkKBzi0qRhPFfX2I5T%2BfXI6y8VVfz9tliaKsO7IS5ATJ8H4RCd2Py%2BvJeaD1pZ3XoD%2FKbiSuqh93jAUPypaTXQJKVmqUMPjv7McGOqUB7qXWW0osjEu9cslS5xU5FWLxB8wOI1r%2FkbAI3T3OWhQHSI0tu2%2FsoMIQa47FY0n1WsZbYYtAnEWfCkAlT4n0LoKORYflkXqzuUMFOGnFvxwxvfExF0tTkeA4UON3Vwmwj1JKXFK8nHk%2FylrV2fEfDLBk1y4Db1hX3wqf1ZW2scgLdW%2F9PkwpxR%2BsU6bPLj4dJLbUnVXy%2BQxIw4GlCzZPZ2%2F5wzlC&X-Amz-Signature=acf6d2529d26006d9cac70894c2b47fb72fad76349913051d1ad40e84e7f14c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

