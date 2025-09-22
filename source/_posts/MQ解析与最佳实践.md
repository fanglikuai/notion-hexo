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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6C4SVRC%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDL509gmZMN%2FfrVWLyjz7xoHcuxf0yII0s1wPGDhfb%2FAgIgFCozIdRR4wtmoXH3Xr8pE43Bi6ehCAas0V4j4%2FVgFz0q%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDCeTsjOyKoXGtFta%2FCrcAxwzfZKvVelxVj%2FZssLHlxGT5%2FaTikCt6mAYcSzZXB7lbA18GV4zip9x4OgRaaY7BxO76jhV6y3dS7Iy9IlE47D9vLqxcXm5F0O8jmcphDi0TgBI6vJu%2F1mpyekA4ONX0kl34oKjQNXGw2l1dJi5AiRjPqUBvEbAcIqs4MLxy9Z0xaacbN%2FQ2SX%2FIvgdTbN91cL%2BsMPMQIMPJ9A%2B%2Fj6F%2FXdpQpTI%2BF%2FSFvh04DUAv2fzVteHy%2Ba%2BCOAqizwrZkTzLtvPSL8lluGtZFjohT21KssUOyEfb8VFL8V0eZ47DWi27cYL%2BY0GhVLbIMuR4IdHo4NHm71NrvfOlwoWWGRtLFcCtVtwiFOwEZoBCHqXWlB%2F%2FbohHdZXGkM6C714hrMPIFK5Lt6pz863DlmNBc8NTCBods4hCmpapUjx78DIXCPq7UPM8T57HFhn8tH9uiYg5d4njASoQ9TpHMxogfeAf3lSrH0AR0eGt0pcJG%2FmQD2DetYyXt%2BHn5ax1AUBqxDTvxhsEgC9Gp7J%2BZc62SK8zVRR2%2BPaU6aB%2F2owlMmT5jqRVu%2BOzEzyPB9p4Rlp19uAzeY25wr3pOuf2zM9Jo0N%2BAZFwKIqJ%2FylIXQ1xTRn43R%2BWIQ3yMYDOtzn0D26MM%2BwwsYGOqUBsKoJwfy0vayF2r2DLlToK7OmUxMLad65waETs2CGr6LmZu5TEEruDE0aR2eS88ZugQhsLtq4rqnVle8kl%2F4uSoYE7daCYlVkLcjZ2xU9F8e%2FdY7QPHBvRogDxR79pjIGi3o3LpNcoeLoR56wuvE%2BhGJWEveV5l8Kmn0wh8K343KeIIx6eq8uwymHHzW5M2JomOQ3%2BAT7eypy7wBdblCk78KgCpYb&X-Amz-Signature=0224b3dd0f4d6ab803372ddcf3cf0ad0e417c275ac0c99a3a58fbbd0abe0b0b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

