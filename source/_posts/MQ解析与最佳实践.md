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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OXK7GCW%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGEh3EgG38U4QTD1kXM8F7f%2Bn4F0p%2FV1SDWJ%2FrSW8b8tAiEA7Ebc5FXHMIKaGmodcJxP6KCeE%2BUs5RpbE06ihsnjFfoq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDDukwRLtWDODMtpWmircA9dzgzXsqDMgLuvLduMye2jAHVgaOdFLakoQtBy9LttypG5uZZrRVpgZgXrva9JfMKj1fU18ghTd7LI55T9oyzVu1cxGDSdpT5MqETwkYms0GwUhI0zpOxuCstkCbB4nBGhuFYHa1eLwHIcoS%2BKNupWSwgEZmntW9tCvf06n32UUKEbtjKXyybq86qLdIR4d%2F0zZRJhvjFtb17zlBG563yCM%2BfhV5TQRi9XsK4QYnr8VuvEuyx4PJq1TZesdDPNcqNn2SY%2BsnUXhjYfgJVtIPJoHFlBBEYd2yjYtqDalVhrJn2KEYjtksI%2FUYxSzQDphjbeaVQd%2Fg8MOnPhy3pLOQZ4NXgOmDoTuuF%2F1SVHdcCwW4APfUQuRJ97sYoaAbgc9C9K0js7nK%2B6yQlsbVWiqe6o3WTybz8mSsgFClXqNU4NfBy%2BB0yuQfjSJ%2BsngpteKPOM1nt%2BU%2B0tfLDtiZU0%2BioTXVq5cvM1z4krTHIFv7iu3pr4rSqRAhovKEM%2BPz0tmu9l7Eai%2B9EkM0t18KVH1sZAlDBSOYMqd2QmReIkNOxMan4PG61WeDOf8UhpikXDfzQb6RDy%2BkBT4R4vnWj%2B40P2GTQQ1JxH46LHgBwbX4zXUOJAKSQpBKx42yr34MLubg8cGOqUBb4kLrczwwl7dbPMlrOy3pHQVyk%2FtbYGOLAc8Cotgd3BEwmsiE4N4U64b6zYqjj%2Bv94vGbTk74SqNkrbmvXfaRzLrKMidBaAUNh%2BCQ44FB2WDzxrkgCUPT9J%2FQU3RPFvnne2lHZdo4nrQsn0I5s0wQnd80hQD9o2Z9yPkVh9UFu9vl9ESSfHiTvgbUvvYjWWWrd9OgmYOh9ZDNM7Q3fygTFxtqbEB&X-Amz-Signature=d23b008c80cc95dfaee3168517115d6cc29262130a164202a14b42c1b975b305&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

