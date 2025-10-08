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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666Y663HCM%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T030113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJGMEQCIHP6m6y0zITWUGNm8qiYMse7Wo9sHALNVCaggT%2BJNibzAiBGYdQkT2BITs8iy3%2Boadxu3hsVmelruq8k81bGKDjXFyqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCRGAHLnk8dYppnI1KtwDgnKXURAONeVdrvE7Sw7hS0wK3FSCms5s39megDizjW0n3dZ329aFVNeHmmKSAgMr0kp2blIxJeWRVKc%2Bln2MfaHTF7oxal2wbf2b8gZE%2FUw6Tt%2FZcxL4pSG2WR2md8L6e%2BS7cK9C33kMNTXa5tm%2B6xXft%2Bxl%2BLzxjGFuj9z%2FIedi4lBPgfq2iGH2LHn4%2Bl3PSCX8Fd2pC8dE%2FS5kCK0uJiaEXq6aR5zePESFnYixHvB2eA0Z56awQwQrI67Ap1NPraDJA9HmkJFi9U7S5i9Ec7TTjJ54kZjzLI4Vrky%2FGGM0uwyHg7I%2B6eVFsGWIH3rYMWczpMAm0gryFeFySOMzvXjh%2Frdaf3EkLDxY9wCfeQW4DcT8cio0dYEW2JZQ3ebNWTLqukhj9KU9u7bz6RBNC4Y6t7p0LWEICRwTQSjbCHowygq%2FVSU2UVs0XTJ2x5XSdjWsSAq3JqI6ARVGQQOJAlVO7YG5Ln8FLUaTVd3XHtGz4WEH4sEKrLajtOOYpiQW7iXL9xx7uyezqfnRmLH2JinYyHisT%2BRxNRVZ3co%2FFiNOSR4K7lZxsxJQESG4BevvQkAUfb%2Bj55Lz9uPiQPQ8bly8EnOUT60%2Blz3VEI%2FkAOpUa9adnkSPPqveDuAw74%2BXxwY6pgF%2B49GBjJhI6n%2B39h63XH11e9UBlMXoPBpHiYDL7faoU0CwaogV4%2FfdnNGhDViu%2FsXAucn1fenYpn5opTgpaHP1QlsJAurYXXKsfvlm%2FFYY5fsVOcDt7SBfMOXqXpd4XMyZ79r8ssfqbx5ErGlLC3BL8t26Lki%2FeoSk3fM1cAptXF5kmf3Pfg04QgSvs5RuarO1wLWUNFOMlygN4VC2rwNdVQlhopzo&X-Amz-Signature=33fa3187d0368ae1abcffc6db765a0d08d25006a79b8fdf93aad8c755034233c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

