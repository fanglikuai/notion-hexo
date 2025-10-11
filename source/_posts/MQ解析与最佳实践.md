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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WOKOIOM%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T120052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCIERf%2F2EG6br%2F%2BlnAJ4Nwwe4fA5Esuz%2FrUBFRYvBobN4hAiBDPJ4z0dT6Fs2pLkq9yrK7ErY8kcoxTl%2F7nCz3AE8s5Cr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMPRIeDcd23%2F3hSaApKtwD7tq4H2TyaYIrOOYBtAq7%2BsOLDG45fFOzpJgL3M7lH5DrDwuVf5wdezS0fcHZ0YaB%2Bl9Pdnd31vJWYWeSpQ6aU4aZ%2BGzOIvblVNWBmmwpXlqYIiX2UjDhlDAk9i%2B3QQedqDn3yFDt4mu2fqiHYd9%2BCiO4cOz4Ww3EUjHdTcNXsIVj5WQBlErSozpDp5Jp98UeGX2C2usWM0TQnunrEtgY0cR%2F5klpFsGJ%2BWqKiMKw7iDu%2Fgp9rRUQ91UCKX7%2FcHaSxRjPv%2F%2Bk43dcvisfaqrgAZLhF69P4TKxwnNK4wENjmuYhDGCxCQHaYnLmPje12XY4j0Bn1NvFiFKhxEXh0uJqQvEw3bv2K%2FNrXCyOYvFfcPmqcTeeREdGdTpQc5Lj42AVETHq5LPXjqSnJqLcQI0zXtve92B5Jmo6QqivYHfV2BKT%2FW1ZtF1X8chYh6VP4uPkhG%2BBADaUyVdyGNCz0vrk%2BNXz4u2%2B4yXss%2BwHTw7f84Fg8ccVUC17BKY9BY30dPtkSRNw42MhpLgtIjAt%2FHalnwqvh5ju1Sy9dyg0rgMC%2FXnGcsUMZo7AwMz%2BB3Ok7NotvfkmmLeQOcbqBe5gYg67QEObtstcx2NJVhMOIjQIbbb%2BolUdLpBM4VEkosw2IapxwY6pgHOzXd1gdtZjdvE4MXJLDqj8oKqqjZn8VSdXK8swrCo1CEtHjzWFaFYTM%2BjjKhmFS73rD4kFzww5co1OyDbN6zBtFtJgpGmB0yWcAggj9L%2ByldOrkgl5fxYlzInUFKN8E1plzt%2FbuWwM9pj1hHjboZ8teBVbCzSsLhb2Pb%2BDRwmK9Vs10UhSC639D7Oc6Wktntiw85bVT%2Ba6ZWca3lhWS%2F2Qopxh1ka&X-Amz-Signature=db64ea4673ddae51858e5cddfb72a41af6e08ab2784a6ec542836e4e6a15e57e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

