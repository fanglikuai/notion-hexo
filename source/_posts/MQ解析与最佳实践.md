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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PKKCW7C%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIGOob%2BJto1HWuYxxBYyLwI09xrsTZapFfBCe8FDvMYoyAiAPC2wC1vpYjhkutiCf1sicYI%2B0rA6ytsHGXviRzCLlTyqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdMSzAkMc9XPv8U41KtwDmfJwn4jkiI4UuQo1Q%2FPCaoeC3MvDV%2F%2FY6M7RLL5GVop%2FaNeLRh7uieCAwudAFMjB%2BblBoUlI6fZhYKpsYipPX%2BEdPK%2FkdDz5AsBqV4n%2FG85aldHHc%2B2ksqEVf8PpazRENEiaXrTd344uf27J6uMN%2Fm9hWUYBvB2L3tKZeVdwlC7pykD7vf0Bp2mbstsUvZ6v4szis49sVdF65E0yRmMJYJkB1qSYyBxj%2Bw4JpbZ%2FUcqSlrwj3yKIyOWwHx0WSvAHjgWpeko2%2FiGppliO68KjCAmvKTcHB5uY%2Bn15r8nvIwt3ivEAMw26QfcXT1QJLM4VBy6Hr48X8pACQCDZKVzw1tpw48r2H8idQoFbZjV9%2BfwvrMoD%2FO0io%2FN8eWJDW6oKn2mEmpDRYx9gFRUwCneCnJFLOpnK8Dbr364b16QsBf17SXxB6bIergC2KbNngiFGdRyBcg2y%2B4%2Bs1UAxBpa%2FPa7frmK%2Fdz7Vx7UZQrRNFQKKY%2BBkIEn%2F2eRvCg8A48md%2Fmtr91ToW0RAN%2F9cNMt3xZyB0VjO0KaNbI22lZuLvnHqr5zC4rJGyQPD2OMMzkgZOZmxHZgsjDDo2Lr4OH71CVQrHwfINzTiXznBqnQWIxwZvB%2FfU1NiCy2oElQw67%2FkxgY6pgFPaonAmPNue0YL6rsRdeMVdiK2G7jstvkY0CJvGriu4TpqWu1ne%2FJjmMiB9ZgwI1ZAd%2FZ9Zu2F6KoWV9HZogJUq5Mz5J4dA1peDT%2BRVeI8D7SWzVu583YYHyUJTpwPPZDAKpg1blo%2FVgK3gQrGIXrDkdmz0udOGQoSvS90y3b0OuCnIWI0uEW07BjNALRptxtSkqdpOgIpseslIjw%2FCFQFI8t1gg74&X-Amz-Signature=0f938091d1e713c09bfa6d8d66c7c2448b41f11181ce3dc2cf5ecbe6502ef4fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

