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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJIKXPXA%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T040051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDoXnZD2fPApahhX%2B5PkaXMjRHndyEzf5hweNlYtZYbIwIgbBGnv%2F0B3itudylFIg3C5OqV1icpHzoUm6nimi6jExMq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDBoTnOc6t1XvD8QbCCrcA4Oiffi5nYiUfi9cmaMDwycZaydXQA655bJk0BsdpvdCI5XPJrwylK78aERlEUj%2FUb%2BQgkT2rRwgOGJW7vi8fSoru3Ll8Xbv354DcFZrt9OBuyyaGfb8ksBzb7cBq6sDzKeN3J1idGU%2Fz%2B6IoTt3sXsA%2BnThyhfZIN5%2BCBqoGP74YWGbWn9A3NwTq9gJ8ysnDNsH%2BkR9ygBaLsZsB3vhNp39Yomkwoyc2SUgXiMiUTc8%2B2FO%2Fpl%2FUdOzjJ2lOhcwZXIyV6dox6ok%2FmQbTfo3s%2B58Z9ubhNUhScYa5kJEbrhRVoLfNLAT0kzWB5%2BiUNm5FpU83lJGa8H%2BTuwtxahz%2FkP7h6NlBT%2BkfvwPyweIGhwGmLhxDWq7RCIJLlbjjoeaxpqhjqsJ0koAFBht9D6ghizjUl%2BN8czcympSKmpLNk6vwaAS4s2%2FsjqBJR9qCEYoRC48ufCjlnSAp7ndIFdqTm6aV3CX3DFurgQQTUnf1TGFa3LkqEkFbz59yZOi4woCa0FtW0pNBJttmVPXRtk0n4vTGE9I5QtfsggAXGQZnEDlAeayrMJerimL%2FG%2BbFvNXwJUNPUBxSJ1mpUri4afGH%2B8g9gM4bKzyTjfgCWSa5UPs%2F%2FQsGStOK1JpNjEmMNDe98YGOqUBbGEqOjqStAy3ceV36i%2Fo7GZWYc0LJ3FxMS4ARxW8UtJR5pjWJ5INCYmjnSGZo%2BAxzbhedfpjan12UyrzYPSRdgxmKGhgMQWbH%2BGvxj8SDEJ1Mbu5KzMumBeydT6Lp3stX%2FxOfb4zsfdKsqduC5C3TJsZ10QSqs%2FFxORrzGe%2FQXiPcLWNQ9peTlTWtRlO%2FAG5WuyN95%2FiP6VF71OOanIYM8xW36UT&X-Amz-Signature=504e5b0b5467553fcc6fbc507f0adbdccfe2cc3435f5a566ced72c178493cd09&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

