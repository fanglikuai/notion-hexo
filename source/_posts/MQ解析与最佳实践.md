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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FHLRK6C%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGIvr%2BKFnoiPqynVrl7JJI%2F6WbqlJSFDGs62GBfYhhf9AiEA5RGRl6SnUHaL3Vo%2FvGddwEjwNDdaamueEfGj35JLIhYqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKZ694HB6%2Fbje2QvoircA1GuLrxb5xqWxpvbsn5mJbnXH7E1L9KwjxTOlXGZQzk%2Bgd5sKJB1gY8mTG8McwMmyPxhPeSiZ%2Fb346%2FKCwBsph2BKHUvE68pN%2FO%2FAIT%2BffSpM96k0jDr6eD%2BKeo%2B1JgpAgPMIXFuzrBI6NK%2BZ5DMHDTfWZrkRqv5moM9iwQr2EsKbJvbpjtcYaBbjpYh7YqRg%2FhLbhSZ6I7kHQMqLVA05IJW0qRgo6YlPsaUt55ukZYHTtUS%2BoEpi%2FIYd9eSg%2BBc%2B8sysNUUrygp0JoOV3W3ZES4xcdfNKWn8t60ko%2FmeVJAfPbWzG8B7rSBhF3BQsR6BLZlLV7nnakwYGO5SGDg3SHpXDem3hwXgaX9VBWEVKzxTgeUnveZX9RyybRK3x8OqJmjyINt%2FxTkCRo8ml3SOg6Mg0ORNIvT%2Fz99nH8eMf51vviwm5RbpxuDG8m3IFVUtkPCpomTOhVk3lBBJk7hANoTMBHuaqD83u6UeDf5baFo5LUTD9eHoV9p5K3XgJ1NzDNpnHL95rn4Z8e8Db5%2FkHL%2FQgrE97D96SNJfvVyJ5FX1Zz6GWcpMeMnmgvVAdnsY8GQXY%2BePi9MzpVEmCpMQ%2Fm1nf9JvdayTZBNm04Y782MQjZVuNcBGfpSPNjwMID%2Fi8cGOqUB9oPKVTiRWzIcvCU4o89CT0auyZPI2M03la4%2FfTbydCqFEuEJDLrHkB9pwGnRvnqNOSNJ32iLiekM2rLH6J0e2N41YAzf6%2F5zkj1WYWXXUqhvFdX4joMqEK716hsMBQ%2FZzmX6SGVVH%2BThwtWrbk70NhyOGZqmWPf2yEyun1ho2tvhSkj%2F7z%2F9cWBi1TaoQ8wvscU2zZpo0BX99FFdF%2FwL%2B8HlrynL&X-Amz-Signature=092aa781c518a5ddf6a4075f095b3aebc11d77f94c9385eeb3cf18bf2f526a7b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

