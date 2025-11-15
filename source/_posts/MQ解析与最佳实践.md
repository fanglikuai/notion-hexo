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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666CHKNZF%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB1udryz702oVhjMPeYiSXdl3RacXd%2B6oeuiPeckYORYAiEAy9OtTswi%2FEVbXum9qCGXfrlhoYzpdtPpQX295cpylDgq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDL9sOxklxSRalY6q%2BSrcA33%2B4Wzxiik9m0d5YO%2FQczEzTR1oaqs0LpLBDmAmvaCPjg8XIXVgDx%2FZIaZqx4Oor3gK2S93ZNs6iBJ4j4yPdl8Ou5rFKQPwSAMzL0SZSO7IwhsvRVse7tuVJPzQPlQ5PLIICb6JrmqXQCAasF5Uy%2FAtfD4hyl9LDgyiwVB5ZVl7U9Sb4UY3hfbuIMWwrvjr9PoHIZwFGeu4jB8J1sHereQaZvokWkGsLeRh21N%2BNXvyNf5amr42ft1uCzIpu2dwQb6T8waSP1aLgB4GPJriUVD5XrR10xq1jhZ9qNylghKLOJEYaY2DiiN9F7GJ6uwnlyEHZrtuMchQLfOkrK6yhE7As%2FbUSqE1zxaBQd7BILjrDZnwN8oIxY2ylFx4JMMubrlNiwVXfIEppRt2w3xiT9yEGXp3j7aUnBCqe1%2BmykUisJKDEvM1ZMcIgrFAVuRdrWLTsXI86t6iW9dxGRQcBKaTr346Ime0%2Fuzjjl48t4jEvMFtEdiWPU2RpOcecK%2BeB0vbCaoY415MHrWrbGYInB0ULvQSna9QmP8Y9Z38QlexDasjqOZTnA78fKcdOWIcwoBcZFfpD36B1zvDhbRb%2F8tr%2F0crrZ6JssLssvlDJE8LQhmdHzGRRmf9vXROMMKQ38gGOqUBn3aMSqHA3%2B7ZIoevfzkcoLfA7J3gBtwU%2BuyKeFZEzXfBe5MqPiYrtoTtjaa0%2FRD%2FxGCmipmEqApAa5WEZg%2BBdbjHrC%2BE%2FkMWCRVaWGikKbeM4%2BvimTQbTxaSeLFOMf1DOC34lXAqY259Swyr7sJrE34RXjU7g%2B1tQbA9lrSTIcm9piyjV8oe3TC6ByT1zs%2Bol3Ks1FEYPqVuBYWqNXKO7LdQnmKs&X-Amz-Signature=20c0d72eee5c108c1691148cd97d4665b2d1a6ca831e52a8a84bf960d7f3f94e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

