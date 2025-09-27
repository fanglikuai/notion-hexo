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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664XGW6AG%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T130051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQCqEwnpU5XeLgXZBvFkEo9VI9P%2Bw%2FTZKZlb7hM7mrYEDQIgSCPyn%2Fayex77r8VLPvmWrhygxgoic788eHg6pNFgt4wqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJKDzTivQ2BEPu%2F9bCrcAys2AiU8zCK3Yg4wK%2FOgYW9ssaK%2BtiKd9R2mXlWhMgGiMUuEjgFveAL4NmKlbNHxZ1BWN9gU%2B3HZ%2BWB9Q%2FU9Rfi75WiqUhMdF2hkOvz0NOf4ssAslx5jS87K2BAC6IUdDZXM1yuM9MYZr%2B64X6Q64bVwmQP7N57L6YX9v3qEABd7JJe7WrHCJKEm5fbqxlnlhPNQmS1sookqQkISWoUvHce91GIi2xxGVKmGlrBd%2FlxHxGiiO2h6LyCkAk%2FjZim8PuvZxjSPSh79r9FAuXWlqRYms94YEZoc1Lm8OUSjVzJwpvllusp%2BerqVQln4RLuGi5CVYyPn49nN3jVdNlNuC5YfTsEmJ4BjnvwT7qt6uOeVbiav1W9Eu6IUOEwx5qwzMqwg0zSNwYulTGu2vaj60hAerGW42NXK8DzvYq7SdY%2BiniKR2%2FXCQhy20VfX0CzrVeir5%2BiySdNZX6TFzA5mDmTgYZSN4C%2B09LeGkpToS2eSdlCohxlguRDIXmCEDZUh3sj%2BQ%2FoH%2FfO2Cw7BKP%2FjEDA%2BCMwyYVTXKkvkKJceA3ryE1gpstsbem8rr2vN1uEoaXYciJbGbKLaG2VgVNVYec8RmlKCAWvVtMmhzGjwQtVREco94kJ3IRR7jtCNMJDj3sYGOqUBBF%2FiOo4P8ImXIYDWBwEax9k9u8lKIfL02jwToxwKvi8onBBPGIg%2F%2B0mr5CvHahskjeWpdx0TTKIFxfie7cgbQ%2Bx7VEeUPOKRyCMA87zaZ2ifAgQsq7aCRY%2BFjLsRzDM%2FU9vwiXzIfVIechgBto6Vm9Zo2jLkieAddqoMpN6R0dR1oij59jC4p%2B5aVaTK7%2FNWovjWUnzj4ovxnwOswjkkcX2d%2FrsR&X-Amz-Signature=adbe52ae577285d31ad2ddb69ae5c1fb3a60529d0cdf57232882eb1f6461e6aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

