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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GAWDISB%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGX981WJGmgaxQizrYeP8AvhMrjVPTGAfHpB3jeEd8eQAiEA8D003GW5Yr1mQpeXPpPaJmLjIuBKxtoOozojGAtNYe4q%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDKaoTaBw6qF1djd8JyrcA%2BkmXkYS%2BvNMnNMli%2BwtCE66zhEFOYG4RoJxZS%2BAbzCuJqITcggMbEAuIAr4ZivYmpBQuGLTtTs1VU1BCrcMTqFJTdzrYRc6gQuRNXeZt%2BWOdb4O%2By3fn2I0a7DnNbcoCXD4CuVzKFJYXPRouo7FKWoMQ33fqGkLdys4%2BXJ44wf%2FQdyXguUmPAcpXtdlneAfw4mI9ScsGlAY1K6al%2FlrykVYKkIg911gblpmiPS0lq%2FTGfxh8HJiSGspwJsogxqOoErpvv4hjdR20UR3%2Btms4CcCSAi9W73%2BQrey4OgJaWHcNgxB1kCNZ1u1qis6VX93uEnpqF%2BJAq0QGjWCIAI6DowDoBZOvpDPJU%2BnfkZPwSZwv1ch0fPDYq1Lw6J3CKIzustxTkO5PamJa6z0dB0a1EcJ0sh8Dll50bcASZ0EIwWgRnit0WG7%2FxcgxHn%2FkVVf2MirUh5HYHz0GkB5nXMV%2BQn7Stl4o4sJ5XLOseCO3U9%2BsSRl5PDBaHYUuHFoTZNy40oumz2MaWhBMNAH8LUtFE4dRRrNkA7TYMYftJliKR3BE0yNl4G002uHW3L4e1Tp%2B2J%2BczJJTqJstmIgSf7Eo9KTRjXDIzAPV50WYffnF%2FBij2wZB%2BKLHX1KFzATMPftpsgGOqUBKUIST%2Fcd7EVCCgbc2Oiiiy0R%2Fu2Y2zMJQNiWNdveDusTP%2FfpvF4Fj9gT9rjyxbtmidub6XWhic60mZnKSAnV05ZitNY766RUdoPh84tCOdZTqgLsaf%2FAzGH9jZ6FY8yvhIN7F%2Bo4XJ%2BjPj0ys5j0yPXJrUaiP%2B4OEx7DRUyj0P9HtWMNJ7DBrnZYmZtTfy1C7yB9dGWy8bhgaRjPeadfexs5VZRL&X-Amz-Signature=7ba0dc3844c12a26954b686d089f7aa08d966fd972a72c377078bbeeba9c3835&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

