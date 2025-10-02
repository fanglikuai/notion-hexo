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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662KBFXAS4%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC8PY79fvP4NbZpTsG8belQUaqJqqh9ORKcPGI7GJq2QgIhANQCBwrDf68OnZ%2B%2B1D2j9i60q38k47XOhLLfInjPUp4cKv8DCCsQABoMNjM3NDIzMTgzODA1IgyfhYKJfZld56tCif4q3AO8rWk%2FLSLM9UWSKBMuLK9xvzRElAWv87OfKXzgMmbjGf6Llh2Q%2F1Q%2FIuxDAkYefVLz5CK3jF0sUhNZWtsL8YHp3g%2F432W%2BN%2BjNESezz1vJ6%2BHv0YC6%2FwizDaWz2HMtJY5qwCCfNAzgwsxoaPaFhqZ6uuaqb6KCNbSEqAjvxBRDypM6lStS3rZZhdiq75gerowUV6cJJ5P2cr%2BjCpqhO2XNmqr1Rioz91gzFL1dhqhdKCtyoMCqMvEJV2L1D%2BvcEAFR9uesdDASg76wunnAZoJmDMUO%2BqQcSMWhNcIXhr%2BCIsetSgX3CpJwD4M1aIH7jKl91SPSmSJBfQQbhB8LIwDifRqzg7gdiC4iVUnpkw1GYZCbXOvXWty%2FhGXFt14gumDI82KAw8oKQyG7O3niEE%2BNPVzufe6B%2Bd20RjegtOPy1UV4BHjgfNUZ5%2B73cPZ%2BLeA8bHq15oEJsL9UZUwZBNsK1Z6tslFWCfgNqKLvDBMZ9j9WqaIs2M91Ajk5RhpLCgZLXeJZ4r%2BmNazJ1ilOV1p8zKNfrooy8dxTr1NBPxS%2BFSXXvym1HE3syI7FiChf3lGZAyMkJ7ugOutg%2BHUddUUzwAMegKE1yTpX5hMzTK4ekjXC%2FEBf2ajGnCP6bjDHkPnGBjqkAZbHJ7QrYGhY5zXI9iztCBo9KqHmvPiZQDE4QiDjh4YL4cki85gf31a32cU1yilyg2a4CAU3BkNQLsAZ%2Bh9f0prpAbs3rqDQDwUWlB1YYMBLvBspujK8coVBp7S6ZhcE3WjVg5XoKPKiKfzZtwT9phy0pG%2BIC5pcwr9I%2F%2FpY9Kz6CSco%2Bb0Hh7AmMnA7kaH0kD382O2pM4RyvxCWymxhWxpyOkqS&X-Amz-Signature=316804099c01460caf3ea5ea4374b2cc0aa582d3f91cff62d2ba25105b6671f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

