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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKGNCVB3%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH48UClHIk9o6uLn9aftRK5WVaiicLx37Czph7UWKZ%2FAAiBMgnMBiZo0MTgLWMNqZ7yj0qMYBHbnI22EKgOeGBi29yr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIM%2BziwFahvKvXSMJqdKtwD%2BMRl8iCT%2FIvOhP9yaGMiS5wXvoabG31TYAkSwHTw7PsQycb97mUbMGmJdQkBXcRdfjZOPvntTL1vTXEgx08j1aMG5%2B9bx94YA55HLWOsvX6Xe6gu1x12eDyisbFfqJ23tikr1NCrE6lBLb0m31BV5zKJM8gFwz2Bxy1XORhz8yqrQYz4%2FGNs4%2F2UYvBQqsnqR3K13mXyggZyZE6AHBt67g0zR5z70Aq3hP2C1XAdvM7ee4Ee8l7Fn2uBeVAVFb9js7L1ATWbKsxjJWOynSsuzjW42HCyWmiRg7BErLTVipYmqUg89qe8YitvG1L9TnfjiytDy7q5Pp0enyS7m%2BtWi1X7j%2B7OUb0iDnJu42m8jF2Cvj64ehBef8aPwUgbBdp%2Fa%2BTG0Zbb8XvR8rrFkdH%2BYPWLorZoQGxtgPfJEeN5yUnbPKD6pc8n1XiEGF6cBJb89sKlxPWd0HhyzrmqsWNDBBlE%2FQAJy60i6O3KOAyrAm5%2FAfaCH5hYlxJ0y%2F68%2BEKsgijqP1Dne0JtV0HVN6mKVgJWRrWgNqP4br0OU%2BTbW3DccwzUqi7sgghPOANb0Bpi7AzfaXQAz%2FtDg%2Bq4L0V%2Fh9IJ2NHJN5QPwdTsz3C7OwWDqjD67bEUWL8hcA0wiK39xgY6pgFEEiSpmz%2F03yddonFMt2tl0n%2B4%2FhrdXRb5NhqsEWVmGhaAuwzFV7OshlIyovzyOSMsteLT%2F2HSUkzz84VLXJzbPvV1AooW0KQW7KY0bPBE6THpf5F7Rg3KzQ2GyvT8F14gwPlv%2Fpwuul48ywgs0hN03JIPZwG%2B13rj4ZIZp1C2tRNi%2B5yZ2vLbD5MPlkROm%2FY3taqC0%2FmE7mKtGbIh1bv%2FeclI3tMi&X-Amz-Signature=90ca5f1be85552af54441e584bd2015f76c3c9694b22af38352cfa7d057d875c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

