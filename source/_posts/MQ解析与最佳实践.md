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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664O4AEQ52%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T130059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQCI4bwGC9ULNDmSaorFu9Bpiwf6BiR0N2J0co%2BEfj7YEQIhAOlT6wf1HoFhpN9CbIsJunudLwBOxvaBwLZbOY5OF3GaKv8DCDYQABoMNjM3NDIzMTgzODA1Igz64M%2FNse4MBUjKWFMq3AOxMXn6psZxVkJzxuF4vINzsUi8YzjNqzkckAXL1ycjRuNbkwqcOOO78jL0NZGkp7VjvY4RdhGw139qIbkpvssphTmj3ppXoROizZVyoDGHU7gh50vN1%2Fl041rhcmxFkn2ZuAv8HOnB7CHDJCzI13l4Dtrq08b%2Bx6CqQY%2FKIz%2BuCxVa5KEagWsYAkxAdc0qdPjVQMeef7m%2FIXG7Wdq5A5mPBW5eQfPZmBSJWv4cVS%2FhesgZGC0L3qj7JyTOlPPLpFlLkZQ0wPnh0gnCoDjVyyJ3SlEi5M6wrkLp6gVVLJIeOGq0zGcT%2FGGz0ktsTCAcvRZiauq3rR%2BRGdKWCA9FBwf%2BjaY4JF%2Bwigex1fAwP44cfuVeFzoLwQdB%2Fqi9QQTmOK4aC27NyAsm7ZkEMZaGnTSA7fo%2BkTFKwQA%2FrJJ7tH1SLr6BM2hsvFZp8byVNRV%2FWxqykW6gCRXEsOQP8BS%2FjoHSjBgXBJQlDNjFZPw5TyNjEJ7Fwignm9hvPjIypdrCWTuewbQYCsw56ITM%2B68ai%2F7lqGDAk%2FU1CdyomBr2gVQsQi5grvcZ84Sh88ffQAuq84Rr49XoIAkSz2Dj4QzMnY%2FVBNhwW%2FYoX8rm5GEVdzJWoyRs73lcW9frYkaixTCKgtLIBjqkAcl4gCufrs2VpQ7CBxMgMW9Nb0GfB5hxJpJ%2FFFzrmY8pRsbGspQBc4CewJR79YjVoxttlulRtb7cahR%2FHHXbbffnRnrXAeWaCv9KfTofwG%2BwpI6YKv4RlJ2H7n2xAk4CRnHIIFTI8ObLhodEk%2BP0qz%2B5atEkVy8oAgdqIzT7Wq8cYVy5XTExke4uXDMWfQF9ZDLC2W%2FMivPAxPM%2FCotC4JmeTRdK&X-Amz-Signature=31f00e1f3df728e93d2b4edf40fe0458c980f269cd9d980685297780aece1692&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

