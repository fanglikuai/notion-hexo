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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MT4CVKN%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGl%2FS2ngcyLulVglnA4E2qGj6w3OjFDx%2FhSGLOxDs3l6AiEA9EZIAO74baOF4AmQu%2Fj7AYjM6bbx3K1WB8%2Bsvse%2BTjsqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOJvlaDYis%2FA6%2BMP8ircA27hY9%2FobQCy%2B%2F0V6Habl9xoY2sAFIJ6gxHf3QjfSXZIfA752GcfzZKoWZtSxkZZ5TwcmnBM5p0m9nLL369WCmYwLJpXbhPR5YlHhqexhJ2grj9DPS0BPLTRMfRVeKnQf5gbZ5W%2FJddjyR6%2Fz0IBQNUgnfkvsH8cKeP5SNMPjDVEXaL8HxSyeaseOPyVBAYLgYhSWOP2mgP8kVcu1Wji%2Fvk5QOSomygDcJkiLhdSjN4rXWkS8WMU0DnZ7Or4wMRzX5lGyhHDucz73ZbKKOG1P3Jtkvk0Kvws7zkJjvkX1XvyGP%2FhV7%2FugpzuZDaz7NhX074E8t0PBJjiSSB6rkIcPU8w4Y%2FNiOYOHP%2FSJBX%2FluSZviX53JpWVPV5Fl2HHG4fl%2FiCiFrWiKkzVqy8Y6jZMwiMds32MAK6rKNmyJvk6eK2cULsTXAZS5aaoCkHJI4%2Bphmfv4mcnTIadxDdGhboZ%2BIcEdKq8AhDdBCi0LA190xr5w1%2BU0OdkxGn%2FFpl4F99IGvksgldKILGrtz%2BUBZXVkR%2F5gRW99cVdUVd1AbrusiyRyaafwwcSC0z0d412OZaALMQAKFz6uWxJhqScGxwh7szx9IEWmfMvCO1rlEfYWtTTG%2FuA8emFmnUeGIUMOnAxscGOqUBaKoHiddeKQYvIZP%2F7pyxuyYGypDgpiZsnEYy2nicSMcrVL0Zjww715Le7NU%2FGxYk3oVqiHMZ6Batez9j%2Bdah1NvCuLrHD6cCf1btCGHbkgn0bjKbxJLUa317ep45P8QPt6%2BprI0yqrmcWFNYMMjqIM9P%2Biz4qFMFP0GKnEUoWVToUZLHIDExKDig%2BMzddMQapgu139lCi8%2FnmbjmsW1Gn5gEpLdL&X-Amz-Signature=71979fa4a2a9cc18924affb2ae3788e56fd05c16e9fb7905ede13363bf37d61d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

