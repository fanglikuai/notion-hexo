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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPR3YWJD%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAURoBQpGw947Ch%2FDIoy%2FOWf6dCAm94Ntze4aIs5oHk9AiEAgKU0tsT0aO%2FUO5WeoEx6%2FwtZSfJLxgJGRvz3uSRvj5Yq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDMjZHQH49ORtRJG8VyrcA7cLEURwiHa2O%2FADBRAWVhyFqNPg25lBCKA5QPvQZX1Iw%2Fm4IAM%2BJWOu4ECekzBkMdx8hLyXGIzy2p3wmemWjO8dFTY%2FBtA3cI9%2BaA0rpsXtsTMegNUqzz4wXwQCWmwKNc%2FaqKuXsZ7FITJ%2BJpYlYn7z8Tn8HWUr4FfXZnLNsOE2jAb%2FSaSI08BIl7YOO7NXEEeJsmElkEbYzru%2FZkrpkSNDWR940jXDMje3ByyoMcrJ3ql8WUVgXrLBkq5WtMb6Ps%2FFMsyk7ghHGDC98GrBALRWkc1sS%2BELNIwzmRsjlaKWbQJmYfkkJJCkl0pkg3y1JoWs5Yb%2FEIa%2F25%2BQJ9ksS8QvmFdYaAp2jlqZvhc18cIflwJh4elXGBVjrOGyO8kVbdSgobS1sM6jKwRnJOYs%2BqLyXkPSO97Xh5VUGgY8IaxgEZJ7rhu8l%2BtMK5C878pUcOmWej%2Fnydt9PaiXuPUxoV78WUfq%2B3rf%2F6fmwJe4lQtGkq6hp9EOuW6UjVOYyrDMQkBDfPrpbnbnBVu02D4YfeBWzYCyiaTJj4W6MCUGYdpUA3Nj9Qg%2BJJQ5JXjJX818Ll%2FBNoNAViYM%2BNGxP33tszcwSv3IlJ2USWb3%2F%2FvKVeYc3WV6%2FLdXqX6O6sW1MP%2Fqr8cGOqUBFOrn3Z7G1jCdC3fE5NiAPUB5AK4JA8G398UEXVxNxlIe90uiimf36HBr%2BlY%2BxdvEmxtt%2Bm02TGoa2wB8IlQYId0fOQXOnx%2F7etV%2Bld9uWnfKONcsnSC%2BkKU4q98tzT9eOPme2dCDB1LCCA9zlMHpk7wXxbjyiyX4B0eOtWg5aYqujdGA0%2FDFBUm4%2F1f8dNoufDoQ%2FKTDaMsfDASYb1yedNgHKwiH&X-Amz-Signature=ebe1170ae247d81d7ff3050ffbed3832d7b6a2d3542841271bcaefef0e1dab32&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

