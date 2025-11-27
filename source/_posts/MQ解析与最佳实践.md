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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCXX7T6X%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHqi3JH74FPJtbV5yvwDnrdy5hjvHqEGetkf%2FIRqmEC8AiEA2JkXHu03COhEaZXHCdWft4q1nfR20PqzTZYb9d8l8CsqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGA5cPD96MNAzhdfYSrcA%2B9GQryNsCvKFVKON8h69AaoLfg3K7lkJNBRkEabp8NG6l4Yep6TeyCN63rcTGXG0sEUcIGTc48%2FrYYVbFGTbyTiA8Pygay1J%2FhBTibEFpWTYJjChh66O6Ez9P%2BuvD%2FczxgglwFieT8M2LGgNrPFr51LYJxH6IT9W43d01fojDAnE9oXR0md%2FC%2B%2FLm4VdN0ex9ksw1QkxMSdhLJO1NXKJZv7WismqgJLPC1%2B25jN6IEIsWPjOXetx1vFKUumlsu6h4VhWdLCOhYUaQQA61WNfILwVQqGQ778kw%2F%2FNqvKz7TUDU9oh3WWuZ%2FZXF%2BgUrVYBKkwtwguo5SpXgwZqhDVKYzZK7zcmyP3%2FIYCHh6S5Pm%2F%2F%2BbABnGxnvCeu3fqfWcvsJlps6CiwuNicQPk4jDdN%2FYQkz4A%2BigGX175gP6CvWVeZRVQAYLxC4cN0LanMQvd8Ba6piOoM6StRCApdnD8DJGOs5%2FNq21flYz2JYqsQV8ujoXj1vKWcF0aeYLA13b0tU96SZvJ6jXX%2BevqSixifsuab5vMIMkacdsSSPwhdnXlXoXAMBDAWpxl4EuSEkriIGLMVme3sgC3DS9rOAZzgdsxFIR5DlEVkuc8S4NGX9p2L5O4VtlMBet3aatRMI2iockGOqUBwbZxWll8RoutxXMK1E%2FEF0HlTOuCwBQYSD4T2OVEKz5sqNcHDMOUlbhPYnwYERUyL0WNfdSC%2FvrXItEmUwKyHnB7IJ9YIZJ%2FOtc2c%2FMVgpdyjR7P1uui5XYmTF1Y9Oi7pBNGRzhPOKyfNMb6x2bT%2FAja2Pe27QQ2VXzc6wXUvCFeUiwViCGVy%2FbCoLAzFbJf36y4cqzFjb3%2FGidfV1ywJon5oGfO&X-Amz-Signature=2d5f70736b0ca3fa626b0422b2de4d2336829f4296c64dffb996825d6b236e46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

