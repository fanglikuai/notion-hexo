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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEGIDGXG%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T140101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQC5Yeoi4k5V1m844BSJenzT0llBVTMMZjmukZqniObWEQIgHAoCtxGMidnLETBXkBLkBi5E%2BQmwGUV7IpHA7H6Q1kYq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDAeEHxmwRllSVa%2FZbircA0IoEmM33UMP1pK7c%2BkFXuxR06J4yhXgxc4LeRq%2F46bOzJFEJUbWE0qVV10cgsJ0QI6GqvQbQqRNCVgFIRUS%2FpLfJzok8yuxVjjDNuVTSSeMhp0ctvHZQslv9r1CraF7EkzcR57IHG81LimGoVty8NMqoyiTlvxxmymByzUOqG6nHZDiWRY8eVPPWpQofwHZgVEDx5N1jlyraceyLHAqMwnJ48BoKiBlUqOA4pwlxhUwC8qdHSURZmURP%2BcQQ%2BZik%2BYRvhQxfzLsQVaMLEL29Ykp%2BgQMwX9extYTxaN6rmJcavTrIFTM0GNI0egJLRA1b7R1qnk7N4AjaLmkgY1GpJN2Ijn5Z%2BWixFTO4tOjfF17usaEv%2BhonU1%2BRe%2BzvVrSLTO3OIredU8g0dZ1397o8F%2FNWjJ5A6Mx04feXMsdcvEd3FDMkHDUKinYnB82dWogcwS2CZV4P3BSDgGhj3CobOnKYmly11yuI7oX%2BBxTLrm%2FuxIOQJBzJJkQwuyyDY543RguIOHi35LTh8TRNYedBMKNEc5RELxYLMfy4pCAzXBV6zhyNkSrqSQNXpm2wIUsDZaHz%2FZ3QJVt1tHmlCfvGGySu%2FDV9QFwnrH4Ud%2BjVlY8GLHObeFu7lR4utIbMJj2l8gGOqUBGeh54rigMc7wkMoDUgA5QFka1BgpLrKV64JVJ55hZ2VgiNqfG1T7FeDjGFcKxdo53Hr1ir0ut1aBWIvG1AdBMBWJPSkYB4bO%2Bdox9J7sDFsE5lFC62ziOaUSrEB5uIxPhLpJVwn1HRhOmoo8QcomYsnt95sSGxQJQ9ZDj5hjggQZ9UTm6PLnI%2FeXV4y1k4y4%2BqE4ek1ySMoMJ00%2BuLB8WE3pbdPL&X-Amz-Signature=4213e6306682cb0ded42522abf13d553c5f1e39eaf1871589850ef37c660eadf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

