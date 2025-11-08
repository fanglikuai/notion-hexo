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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WXQW7MI%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T160110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGnSbvBMxsZTk2NHSVLzjmqWq9Y8jvBMUQJhOcxXxEZdAiASx54lX5PTyaKmnNe2sJWKtF26MA7VQ%2BOqHvsmRas%2F7CqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIvJQlyKFHdCHme1EKtwDG5Z0MhttGPMfCoIKBRb4iKfPE9MSteQq4jSkmt%2FvtvEIh3GylW4KvKQcOsUnco4afE8vLywhdg10NTY11K48siRweN7DS%2BhhhNuZob%2F3GrQNEwYlcMo4j1aJmhswqMrwsonkXjL4bZO9kETcbfvJ5sSLiKdqx%2BgV0KtvPkVIGsflhJdk3htfK2pDcTsnqUhcTt%2BwxxkXT0F7TaDKTCMzKRK3ACQ3PMkITDqil5jixLKRpzyni6z1wB6tsh97LjR3I1OQ0VoXDP7qfuIMGU%2BnyqfgEeRiUqP2kVNi4mYxkU%2F3uSuvl8ukd68vTp1G4iAFHZykGpVtXi90MvwY3hfbB7e1sxsKf82xcegXZNfaFkfDmAUhBJaCvR3HW2oDrH8Z2Ds66UvKp2DAlT5xXms9YtBOnAzSK6e%2BKwhNCfCGsW8USQqjOxew9Qzd%2BUvT8u%2Bz%2Fon7Ab7f6fa2yGtgQyJMNooql2Zs%2Bb0JGB2ITSata%2B6WYk3HKlHhgjMOICNTp3ix6L2zLn1PPzPv%2FWFddOzcD8QB45TTcnpYJOsoRQqU0YgkBC2RTkEwgQ5SoruXK9Qb7AhXU0%2Fi1Pu0%2F5dckZHZ5n%2BelVhdMgOkIIQQJBidaZuSWs%2Bse9F3LpnzaCow87K9yAY6pgG9VXEslSKwrGzvi3gfQVPZAupNo9AMuVFNUqTntsnX454C6cJ2FPZuAa0xARjEvrusA3%2Bsz4JIyrhrUc0TXAZ3lfGtWaLO81BQz5yQ8JHSXVV55z9IQAnuNAiNMLfYSLPyqCTfKxNUh04PIrepzCkk3M2TnmkDdpNE1EdDDoohbk0wxUGMi6GtZBdOzwcX%2FLoEOx%2Fl8YzH9BDYWaGsJz4OuCRfw0T6&X-Amz-Signature=ea7d1c51471626fe27d5bc9aaee23ca4ffd027716e988c6e79eb292816cfe5c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

