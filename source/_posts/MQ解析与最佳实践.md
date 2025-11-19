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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSA4765Q%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIQDr%2B2etowqdAADi43paJUoSO9L8gK2mwin9923N%2BMyWhQIgHvUtbp%2BYSeJRGNJtJgmxzsWnZMmSmBIM1t%2BD8VgfEVcqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPm0NKDOz9HZtcY2%2BCrcA8JVWxJJzHUPDwXn%2BvD9uuQTcK2cxDZcjtQtZetp%2BpoWVXl5m8Z9i3eHAKPt%2BnuJwKBKzlsOF1HRYYmfokImObGG%2FqQv4NSF6hZ%2Fst53%2BnZpapHrIKEQPsu32KcKcdf5gZIL22SWOnjrNs%2B%2F4okeSn%2FE5sz%2BazUx6Xc4cPcomQZVf8gVcm3S%2BodEBH4WGttAs%2Baq%2F1TtkAYQGaPj9Cs7YsusykuB%2FmA43le1t57bKcuvOFk21bNy9xWBuIoXTgGvBQSTWGH66tIaXu9M6gfa73aTS2GCl6fQQGo%2FqD%2Fdf%2FOASy1vLtWuKTjNMrS5YKjqraKYf0Yx5%2B5yANQXjBU9L3ZWWvAZZ0d7OvNPTK4WVQK14PS95%2BwX76TEIUWWpiSU2SMG%2BabQUDaChGEUqvUzR6bffAsSIjBA%2Fs70qx%2FHthci%2B2%2F2qlMas2hflrZLom0jszpXgtqfoU7D01rw%2BrJZSY%2BZ2EdaMJQhLGqhzulbzd9Fp5jRMO7EziEASpySRIGjag6F65dDdrvwEvoCCT0IUOSk3WL%2B7vzpIE0ow9ImAzxc716XfTybsyUaP0FgFO6O%2FyusR32lbGYvGKTb%2FG7I6QrAoA0BIp2IgTJ4GoFjWZm30pau7An5REmJ3xi6MLzz9sgGOqUBxOoamyyL3vgWvE9%2BJGzfBYlf%2FGbcP6xomGynzitlb9Kxn7meH2PB4HY4Sr37ihi05jRAfPP0kdT5e%2BV%2FT%2BXkouoDo2AdRFzrQdN6iq0t6Q9vM9IxoCjJ8%2B7pjXDTYuGgJ2j9PpqZGFwYVZC%2F3S%2ByOCN0cN8CEjEIxTR6JWRUbAU7D%2BPKrV7974Hiq2MUwsOCHsN9SD%2FPY5tUB0d7UFXs3VAJcK2%2F&X-Amz-Signature=f52f718a3e21ccbfa7a3da0ff86f51e39d354acedeefa07663d71cb9f717f0c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

