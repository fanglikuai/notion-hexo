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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GI2PWPH%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCICw3OpBZsXPhmUsyLsXODr2F9bNxTIMVwvxLpYHbpT5sAiBvP23lcOsFvQ2TV3A%2BiR6UnsDDOR4RfjQYxz%2F%2BxofKqSqIBAjd%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7jWrb8yVL2C4z%2BL5KtwDPgN4Y2PM1xCRJVNbN8t0BvoHSCxVmT%2FEQUWf%2FDPRPI1xSzez2rhf59%2Bfhs3sUIb2o58KWUhfOJuxVmfk06sn5qQ56z9UhneApdpVlaqawIbLW1skOmNqo99TrhyYeUCnkKHAqYBDfHfxpiqg5jVhSzKi5PCVDTYyFYlAX6TKvdyGgMUGCtVe2krFmr9661TPjY8Q%2BhuBV0aFqz5YwdrjYq0Vl1hsPtaPF7TwHtPUM%2BFxenN6h5fRVRJYh3L%2FSLMqiejTIoHLpHEmGy7radmlaeEqkmuTLKZ42%2FOaOmlO3RwOyO8vC6pGgdf85cnMwuOS3m4pvy%2F1LRfKDpkGOnEsCAoJHNDnZfZ4lWzIcyXHMJlRVyUMVPi6mdXPUTQki%2BStDPiHDwV%2BhlQcGKhWQWnL7M8Xl390WT6ZKwkOyHlBlpZ2%2B%2Bf133b%2BDgd3bQrl8YhH2p%2Bf5bGEY5KXyeFo1Dk61TvF1ycgTsIaRcdKk2H1WHpuKTN0ZQshdKkz7Lr1rSFYiTTD29H4bYJ4lT5O0P77tjWLsO3I3nzac11kH9XvUBFn5YMd9sgHE6z%2FQ%2F2jz3yTYaYPZviM2Qn%2FBAqb5e%2B%2FHWo3JOtRTmOgmgK%2BGVcbrFM%2BuTrUoTtP%2Fg7iSmgwu6SgxwY6pgFtolR3LqGCwm4FdVwkll6XpqzyDDeq9ppuLZuKb4TQo%2Fi5Efr4ngaGBdtzqdECRLg8ItdgOutf4ofdecDJwSe2o2ZMHj4Uw8x4Db53rsu5zuD97ReOKNS6eDpde7CyHTzNllxjrdRq%2FlD%2B%2BJwL4IAz2eRjcwGV%2Fl0QV5NgBqVNX76m2Ay46mnUIprXqjDblNJod6tAH5ZQe5DgvqaRj7%2BLE9t47tAd&X-Amz-Signature=c973c116561bb330535def94474ce2d72e940d270b7b450917ceec61fa231e99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

