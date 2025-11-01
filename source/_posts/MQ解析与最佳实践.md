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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622LT5U6S%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIFRmdC6AL4%2FD3%2B%2FuQcqV%2BEaOW2aMk8HL6KMA5eeG%2Beo2AiEAsqdXaJnGtu1ZZKQPUdTlHUqfitCUeXvMzl4J%2FeNuWhcq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDPx7D75deUsDBkLicircA54spyqsbQEFMQXu5Q4h36KjDThu2edEmV0rMQ2oTFItpeSnKCZTAQsl24gWuqXPNYG1G13FtxDdtGjsTTN49IETabtBrxz%2FgJWoBjSVkQ9UhbLhoefjSnjU3YWzbpJGsn%2BR6Q%2Brx3aVdBVwBrfstJdL%2B1Ft01E3TX1e6f%2FW1YmN7zJz%2FljCUhmwc85%2BocJcUSPgX2ich3lAMbHek14HovCEa3oCGV50tTHCm4v33sU%2B8WzmxJkp%2Fq6TZVMC%2FZFfOvstCoomCZ8s8zH8m6W5HH4vGkULLRYa6p1hK3Kw%2BdWE5Xc61cKTTA0GYlOYBKZqKYiIjDqbGtPuY99%2FXOr1V6e9lQG9pp9rtNoa35qkix%2BExSZRZE3uy82wik53eG06URLSjnir80MIMwjdvwpvYXDKcN9FdP5rlhDrLGPROefhW%2BSs1wqR%2FRTTDVWZwNkPhzbx%2F0n83ATMJoD5MJqOjvD1fm%2FPDiohM9UFoFwqsAFEWH2ZjMWhAGritFHFNXn%2FB7tNCigovD7lhlgGhYjuhyLik0YXPO92FKz9V13CAlYnGV55AkO5UZSNxGPTdWeH7lGPVNN71vfX4NoFSVcqPRTdsyjKtKHr4VBJlyjOkXgKdJQyhgiydOzgj2UjML%2FQlsgGOqUBwRvi9M%2BehwF%2F4f%2FKHiYmz7TwHmFpvEk78M8To%2FRl2cZg1kW50v%2BIQS7anryQbWD4Kfv1A1qEq5fPhFEsMrIlRj5%2FZL3Xq2huB8G7yrxBJJ%2FOZvYMCZ%2FoYoVOlUUXCpWIV%2FKTuo0XvsX1%2Bzwu9OdpnRJGF%2FhcX7qMkWQqlIYQ%2FBukaVEtvBEny%2BLTgrUiqSjFRRvoAWLxXLMBtGQFWcb5QOSUzczu&X-Amz-Signature=360818f1084983769f6583ab7e36c46a828903948f2e81d2b2ebd2bb31f0a25d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

