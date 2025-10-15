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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HWITDHK%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA8cutE8BAo%2BtX4Jf0KCkVB5BUYe1Nwt%2FaWfldOWVbv3AiEA27Kf9ewKYpHQoVzl6H1QHD9zwB%2BzUcDPU%2BWGxiSS45Yq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDLKsy0sFQP%2B30oXguCrcAw9A4YzyfiW3Rsj7Iszqn4lgXjksan0ORJZthODpjvPWJEUxFl2Q7SVbWmwa%2BbGQdjIOxW9psV0zb%2BXo0bndLy16WyzzIN6KMXW%2FB44M%2FBf20jvX07tTLXUjlUn0IHu5%2BVDQbvRsVAbYyQqu0F1xG0FnIZVqpcq69iiRvciIU5NFoR0KfPoZxcSJfijaIGCF3Vp2kZYYFxzo1BEhXpzVxHnT1CsXEe6SREIPqJsWq7radqLWf6ZFIaskmXJWGJyJqpPUJJXv90rPBcwJ1WmZK%2BPvQRxJRThnKwJzXHNLzho2WlDIRCaG6ntoKiansaLH1GUH0qDWbdeZziq4ZlWR3QR7DuXSzR5BnGHPzBwovafGv0FP5SnjPQzmTgJDRN56bl7tJRbqTZ2ZOPqqNdR3EycG6Fhc3ZNFnAgYi4DXT3WYK9Y67cXG0T3Fs2eoZHwM6qtvwD2RL%2FpIMfSsXWLQvGqaBqI23yM8QLyAJJhe3NPWaJaoEeAHAuXLhDJsh2WNnQKDI%2BipM%2FzigMptOjDTm6Ja8hU4saujhVSRWB9UIrkSiN6v1Qfi305wZzaouiKSxwbl3yodTmn0C0vcy85gNuFJGt5jTHW2daheKgXaYPEYpi75%2BDI7KUQIxbZ%2FMJLEu8cGOqUBWqe4h0vRKPtrL10ZUntb5TPHK71jhrrO3UUJ6GWGkRvIwL5%2FSWKbKs5efCIc3WFA6wfoeubhWva8gxbceasv4%2BKNX%2B7AL9aKLy%2B4tq%2FdYNijypC%2FgRgeFzw79b2LMkx2oc8foVXQmDZJhF9JlxbLh8OX8d4ChiAUGwv304EHkoDoNDP%2BqYUh4cchTrSJsKhX7sXNr7FdJ3quFvE128%2F2FmbeRH9Z&X-Amz-Signature=9156df920d7ee9af20a9e987a454cd7b687c826f2c4849b9082caee6b336d53a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

