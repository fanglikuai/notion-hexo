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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DII3ICX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCICCgKQEs27Yap3f5vzAMA7qQtpfWusgbsojNxd2n02f%2FAiAaiOAGvEJPqB9AkcP3vTaAi2JpshaR4gJW%2BJ7qhqOyWyqIBAjl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLHO6o50r7rPjj%2F7xKtwD0czDOsMwe%2B%2BPS20jaBhiOpRt1BcfkfRuL8OJpLTW%2FVmnKUJg7OxMJDbsEJWW10TQPFXn%2BxLo07HnDT12gOk2uEdwgfxbe8GZ%2F%2Bj%2F56olwrcJaEKG8SF7fgeatrW%2B5HWFr0bCYwH81qGspb4P6eIRoAQcKtVGkqQKcLbIbJetV4b1b22s3v1eg9e1mq2pFauk5NUOp7DJd8DYr%2BR3bLBuiG2vJCyUfzT7gQmEtkgxxf7N09G9vUEU57VlWr%2BT6pP%2BiQWYtn3k%2BDvr6Qz8LcxyEoWHF%2FsJcSFfFGipLKqS6tKzDHtKPsSUsLRxtDbMROuyFWppi2MRG61DjBopXCv6d7igwNJGnTGirgQBz8tWhR0UjrcDVZ4pMeGTT8fYGJQNjTunY%2FWIWsErG6Rp5ZEF6wuC7nfebY4n%2FssjLp5PtP3SU2KKUMYorXFW7ojqQZV3MwXY%2F3vLtx7gv2GrnP2l2uReDDKe3P%2BjTzfPyI7NBZoXWUr1SIgbHmNa7bETtPA8CtoUKhtSGTiLZjXhymwUQ3bDCYK0clV6zBO37%2FVjmWjzYkHFImn4EyJgZChDunVqpLeIN8F%2FRl%2B6j0FT%2Fq0Z7SGVPve%2Beyf2WeJZsBplZsm5MxooaCXGQ3bnnkEwrPyhxwY6pgHdpu%2B6MpAbt1jkjDfOUPhvOPKKobUbuldXy2fPysc%2B0%2BFJK%2F2vaGPEmT61wBnSP%2BRFEQ0EZkqHAmve4Nf8QlT7DSffTYoohJ1DC9dg24MhrxhogIOgTuKsgglZ20JWMwpDugCxJlq0vnPsJJU1Ojb79dKfESk8%2Bi%2Bnis9HU52N%2FEHUlzpyNAtCiwA0qH6yLmj1JmZZT2BmYZmLILqGAHef9%2FikoEGb&X-Amz-Signature=4cfc85826e62c1170a98e49acde4ffec97309b3a04c3d168d0e93979010ff3af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

