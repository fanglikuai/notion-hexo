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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDZESLIR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T060140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJHMEUCIQCvq4JyN%2FxH7obRSWD6qUqW%2BwOE5b9yTyRcZUCYa%2Bj22AIgCbZSv1RAJw5s499Ktvv4teSXqoovYfMPCvuqIm2X5vIqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPooHboPAvovkYAPASrcA%2FBTD2YQSstk%2BGRVH03I%2FhvM6SL0ztfkL5TThXR6cUDHEcsj37zg05dPwcUzsAENNshJZ6LYBqsRwYwOR%2BynGBb5hFQp1GC11iP5txsdsvehQWDekC4ZJUkIKHYK2%2BsrP%2BWlEZltWAO5e9XrSzHdhhrsmE26EuYhAHxYZVdZtfSqyg9LDFdCf3GQ5UNwaSfEPCJ2HI%2FuEwew4Yi4qAqbpaAwDJJF3GKowDigWZfCYPCwFEfWQwsSdDzdfo%2BimZslzByhZ96SkfPhO7a9268QB9eujMB5%2BRBfPIioUPJJU5%2Bz7DtZtqo%2FzV6BrluG4T3GBYt7%2F0s7uMuAyAWTRbtDjOYfjDPDnN%2BBQ13w5FXaFMVKK3ubHwI6av%2FmpVYJwp3r2meEYL4j4fo8mc0rbFSGvRMj0iZa8bfim4NGtXNgsbJCG9lTfuTizj%2Fydk8W1GAXwVBWwQc0S66PN1Cx7%2FZquXC5Uq563rzxM1ZDacBa%2FSQaZHcSbUhzWiVUmxv1C5sqBhueQTGHIQLBT5mlBjVzTync%2FU87EoammUdGBdjQosjWBy2YXr5%2FAiqx7ChHXSa1GhB%2BF6UaTYYcvsgqaGu9YvJjUHtm%2F01J0subgUAE5J5D3GoTR0WufghjInIMMILSkscGOqUBp8LyOxkWoXLk8aXC8QQXzxpmtpALslBdJ1FJxerf0wzfradD5DDT5IEDQvWC9V3Wn%2FqeR4HxMBAohlvCGpdR2BL4hSl%2FKa3gpsSRB20oUuZgfoETHBEm6UCf8D1N9JGMP%2FUkx0fcn1v7%2F8IQDHrMtO8f9C6YDnylhMWtjhOUBieKrlmMIRhFDw2qRu7FaJlaNx4NTrlbm538HM96DqPKqfM2Pk6Q&X-Amz-Signature=cde22f56db491ef144dc3d97761ddc10cc6789d6a0da6573f2ea89f45ea53d5c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

