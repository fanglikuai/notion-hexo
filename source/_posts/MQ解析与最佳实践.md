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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YYB2JFZ%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T010052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDUvNInfEHOZamrFD%2FN%2BjlqYy9zC5kqYpx0L0%2B1uuuZfAiA482YKX9iC8%2FD6YaXH8DoyL47Dc0N6AILWhjkoRrzKgSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMH5QlWlDSh0eU3GkPKtwDZqvFKZCDVbdMEdvFA1%2FrHDvPbgMKcx4FglWayr%2BrCG5hwxElZqorzkHBH5x2spVkZNuTgk9i8OiFSPXV1QGGHgPQ89cke2HXmBxF%2BEAqvTrVp8EGN9poO%2Bfly%2Bu24YmVnA7mqBe2EGMTos1NEUY1agtWujt3Dz6QMcPNBuEk2VPyxmDpcGmmAvwZCTvcsL1Af2he7tj5XdAjFyV5PV4J88cfRlqRHCtdzGc8TlgjiaPu21a0D7WQnly1r2ntEpe8p14OR6wD5ej39l8u0%2Bmv93fDyCz5xMo8gpwmJUM08AWaaxqtMHqyF8KwIB9kltZ2INZ8Z%2B%2FmtbEyec%2F0Y5nOdr77qZP7tRpqDRvJbGISuFMX3QkDjXN5KgWaUBtdx0mRzbvFDevAQFAXDl%2F9baiC1omUgrJXAMkjXZpdBb9ajOAiFOypDT9fjNt4hnEnkN7QzPvoM078t1fr5I6BJo%2B0ENtOz3ipsYtDwwMtyRz%2BmEbj%2FFFHy6Rku9jla%2BQ9I%2B61sRYel01pt%2FfJ8VAFVbL9O7pcqffTotUGmZXsw9O03Cn23S%2Fm2zGgK7HXbSCJVJDos%2FAURlzE0hGiGd4ABo5mfLOqQwWFevAPXNrzV%2FbqsTPH9K07AO4LF5%2Bx3Ycw7JzGxwY6pgGusz4TJSFpLYzccdiNoazb07%2Bz0D6L%2FD48Y5yls5%2BojSb0mXd5vhYi65P%2BAZIWhDdZLklorsBPuOh2iXEpihluqtmkjR7eq4Ow27XaKpnCiSzYjdiVB6MW1X7sbk3eytYaS%2F80UxIn7QqB8uR15P%2BCVh5yCK7O3%2Fw61SotCfezpHrh52xYFxR3OjU%2B6QspQFraV7BPhEezKRBxYvtKBgIT5MjyUfhB&X-Amz-Signature=53d07eeac59723e6591a33c56daf15612d497a2b6373d6a5f0f8832f366afe3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

