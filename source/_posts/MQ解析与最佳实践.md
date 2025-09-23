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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SFUPPID%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE51cv91Bs2CPbPNpka1SQex7xJQRrmDz78xJtKQpekTAiEA9EiGW0xepoOjPAj3Joth2AUjrj2fInMzVwfS3af90hIq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDMJck%2BOVjunfDFhw4ircA0wQYsJJPYBDn%2F9h0MG7cVqRnvcXx833wCN4XAun5fQJqadcDXFmnf%2FtPFpnTefEhh4%2B33KzCdSaQ5OKEhoNSzjltqDbgir8643E2E4v4NnrPghPBTRBk7YjhLvcvltNDcc9B6Ilm0kV2Ytgbt%2B3z3V1RxPzLsr2ltrkwvKlmr0EhuB7PhfKiUnBdTQY49PlAafr8M%2BD75km8bZ8wSrVStg2QGyIAH5qdOQmKwPAqtj8s3rRbuGjsciIaVwZIiGf9c6hKQpCv0dx8NbM792T8Zw0qi6x3LEglq9T15nvitgTXZozNuR%2FaPDLCX8sJtd7bbgjPuBBpOjlfLkpv7gcKcdFQ4HkSKnDvLf5UuzeB9A3Bp9DK8V1QuEt2P1JzEmKhfNBQL0e7T6Csw08%2B13NiP0qgleU2FTmryDyjUxD8VTYdtE1tij12JdJ7Hhh7nkxZs4KbF8V8Ols6HOEThoYVV4Rr3smPw8K9%2BaLrNNjBKb9u0MUp0IoBHc39YZXo%2FuCgCLUmTi4%2Fe2Tfc3rXfrdaDo2N5B2Ie9pZCPggkpPZ6POGttiVXkrwPGW5GLZUNxax8vwpv8U763Ds%2FWxG%2BZkGVd6TuGe1vEjBtNnZfOo85FAf2yqP%2Bh7gFhervWYMMaTysYGOqUBKKER%2F0QO9S8a8H2LvJsgLNM%2FofxX4R41Kno%2FlsQkWGDRcFOpmmKs%2BxyNSs7nhpDgUEX8H%2BxFcgicQu%2BjySZYmcTHMwqG4oHaqGG2hIfQPn6cpmZRz9%2BQ9kGc1ZMV9VxnGIwSvwDzpKpzWFmB%2FzneJH%2B%2BcTSqmirA%2F94eakqojdV2Cw6HMUgbLtXY%2FGRw9CEKwFKPmuOhkTMu2LNgg7omsYKXwoVa&X-Amz-Signature=7c7155336f6f53721832847acc718e922cee120d513d502735029452254e9c6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

