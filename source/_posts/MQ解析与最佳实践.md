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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JEI5GRK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJGMEQCIBTA495SEbqH9fqBudcujxJRr6URpFuKf0UF%2FZimhs%2BBAiBQPpMCx5eTnGQmZ%2F85AmwrCLpTuo47SKt2nCcN5viO%2BCqIBAjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDks3Rb6w2Lvf%2BXK8KtwDTvTim8%2Bt4V1oDfn2OlOVYkfKZbIzDpwKqI6zdm8FyWjYyouf8szQRhIsYkFmkC9LoRwRd5HCs951H071mSPGnNdn5TRNk38Tl8fW0pqxLNW9tcc5gUdzpji%2B1G0bAlAWXao2RRhbG1Jcsoydfb0F8ce5eHhYrdA21J0IQJjq7Kz2rTjrIMHD9bHyTi1K78Z9%2FgqKnb81vZm0HXg%2F4cLBuAcm0GrVGXeOszbev9z02BJgqvbfmRVMa6WJiaZTdL7T5Z0n68sW3kivEY8oKML3itXMemnv6RW1%2Bjp5o%2BIlPzfX5ywUZZ6t2qoO0RFJxhxTXmAD7V1Iz4%2Fne4mwKXd3SzFZZ2slKZPbenUpM8TLj%2FElS49n%2FxsP8%2BTRqZ6HAclkPXhtc7OI58gl7AhafvnBecT61%2BAS76Oy3QeJypZO6EvDkr9KcbgMeNTMcwfyR9nj64qyUAw%2BJfIAYX1sWfAKlzunjXnT8LCZbHcHm%2FscSKUiWY6k3yR0xQ3IWd8DPVcs0gkBw9ARhca9waLJKst6aWJVLZj%2FILrQZUbgFnyHvh9YQPlMPJZIs7NLR7%2BGtdPRCKuyUkC8H3CijsjRlAIdKuY1XaLZtjYAkMLnzkjfRcvO%2FsqPTiXo9CHLWbYw7OSgxwY6pgHxeH8ufMC8Qk2eIfShUM6ISco1jsgDYygUWPRsGxQDKB6BV%2F1jV5TEDQG%2B4VQW6HEKAh9VidQ%2FnyHvKJk4XdjKSAeh6l0Pdyi4EIpuQ6QhyyzNANL78t5XC7jGh3aIhyctgyceMjdWoER%2Bof6hynmpxZbBim4G5qMxr4MpTdXrST6ncjMjGLRElH3c%2FwQnW8QgV6KlfR9gogX7jJKqlZS7YJK41e%2BC&X-Amz-Signature=7965d55a3f208ab0f3039dc8adc4b271f3f7d1bcfd681274cdfc3579c06ca430&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

