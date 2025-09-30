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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6ARI7DC%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQCu5XCb1RKbT7VZ%2FAfOqOg45UwG%2FKmFUkT4Ff%2Ba%2B24EtgIgHe2ksVJsvA%2Fd4hUMs86%2Bwd09UyYMAOQONtRpKXIMcekqiAQI7f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPOP0%2FKlRGDSBgfKKCrcAyOPyceIXMRNMXbT8TayO18K9gSW3iuLHmpFRW46Q%2BtfxQjNxt%2B%2FrCvi6hrQnf1KL2SR6iXaLrQmzO8bTUdFZ7suUOpZy8ObDtx6C87JLNORvWi%2BIBi9Kak9tAG85DFacxcUwCZZ4O%2FXC7CRl6iVt1ZgZpqq%2B1WRIsuXb4eUAbsLRQkeUsomykRDKsr27ZBpwiUJDgn76R0LSAuU8nW2v4HaScUSNApBN2sZoHYpBp7qpxrxFXXkKhQbUoc12pS7BkfIF%2F6OjjqxwQY4EFDAqpEzaWDr0Y2qIlS%2BkKYqGAs9XuoSlQlr0bL0csiHCoRGHfsvgqyNQLpsWcLXL5bfxAN11efOCva6zsNgQ1lZmsBEKW0DACsuJLbyA9JAKb3J7ONhupfosTC8Ar00qKp3zIIkLh7jLxeDABQ%2B%2FF5WOygjBpd614rMQ0q7zaUuYL0QEwZkwyNWNf5Q2NrTDg21PssWafXqGKsvZ3KOhn%2BhjeQg8%2FJSJzGEFUX1oO73Pr9MMZqEL%2Fxzgj0iICaXnihRTb%2BtqtG%2Bvl2xlBd3Pr%2F4wTlwSursIHCveInvu6ndT%2B3SNAbxZ4IO66nqQAeZoUB7sLP%2FJ9ukldFwBI5vQNf%2BrG18RS7wmM%2Bu6cCAk7m7MIOL78YGOqUB%2BFtxqCG6C%2BZ4tYrxmKThdkHXHKtiUCcW%2F%2FEuSxSi1jy%2FMv%2BCJOS9QC8jmmEwi06ok40zU51pLFKCwR%2BnG51U0jcNHPVonaVXvEW3gm04WzcAv8nsiufXqCN44NCdmH60cChQOOQAu%2FV%2FgrJzTxzc83TU6wF6vAzLS0IFn6mG%2FzM6%2FP4vPXFKAFRuU15JK7pQnKv%2BVmorRQc93UePOFhR2IYsVUd7&X-Amz-Signature=d6c33a0604905dccb2576895bd3a9d55ddb717a9d92d872857daf787238d4033&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

