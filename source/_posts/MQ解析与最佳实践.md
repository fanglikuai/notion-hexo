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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNYGCL55%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T100038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7GErKPnw8nvs3zWVAxZ%2B6KsEYjtRgv66GJRDSOBA%2FAwIgIkZQebgmteZkWBDK0jHRsEES1mFbpkemiz7ZM1av8EAqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKOV1TZxBbPsv%2FDhQCrcA5aOMZ%2BwTT9qxKRjsGcpASIbadr08n3LxwxXsf%2BW%2FoeRHAK4WEE4vZj6Ij5WbWW2OeJxJCBIaIeMbtTfTKo9nvikmwl6dkT74FCWnJ5yXCQ%2BJ%2Fnf2DPUypjfSvwYIbzn7xD9tWNEU5fOZHhgi4lcbPHwAhjs%2BzwYXJAhd%2FfcmdsPCCHzbpwigRuJM%2FHG7jOPhMxcBWfrn1WL3yHGWp4R1gKOZIZkzeZZTH3MY5Czpw7XTGxBKkLPGaluttrrCyU7oxtdR8Z9Dr69cR4dR0Ja2aWUz5%2FyYb5DnhZO5yOh%2FyIZ%2F7HBh3G5ItJf%2BBQQI6Ck0Gp1LNEWcEMUB%2FS3iB1rDo7i9NwmKqI%2FmjSxNn9EMCnK7xtn955S5WmPaMCAXdvCTWo8N3871hcnlEVovcKtfRDMiHucrxxvMzWPToIZyeVHDzFUyIyxrVmt5fe44mnN4SgGggYXLF7leT4PLWUAqjg8BEGvzS7Wvre8IOi90gEViaFIQRdO8gdu9hcxDG0p897om5BhG3qN5tImWUlGEOiX9O43m%2BZMin%2FOL%2FHCc5uwbMfWLYZmK7RrR8ococOzSw3%2F%2Bn5m9mjtZbJPgWw98pdH%2FrKOR20EJjWbcXLNiGYcy0KrLdil2plsDw1iMMb%2FvcYGOqUBZmQgQXAhvBeYhsJ0Bk%2FR2AzURv836R7rH7ieLzF7GMP4ESMP%2BAz9cTUdbxX8rHpJxVsmznOqrpDHrBHHqp%2BXJBsIinBoQiJCbyqvyRO2Ni6iejhwYBgCruhCcjiUv4h2JLxeouP70S%2FYZl9MxU6arFqorkDNy0bM6kCJRJpQJv4U%2FobAXCfqev%2BHzbmxD17qWo8LvvDsuSQAjZ4tLSTpsMGP87iW&X-Amz-Signature=17cb850843ef4fdcfcaab62e39c4cd8694d1beecb989c774f269473c5df098ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

