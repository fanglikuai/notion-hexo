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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRYL7M2N%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T060056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDk2%2FgcSFCCuw2ZpuIktyKh7gL7fGt4y9nueVaHnC71wgIgMRzIJUtR49VRRjbHaMTQal3CF2cg%2B6Wl8z0jg2uXuYAq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDA1Vhp3P2NY3Qj7RDCrcA6GM4PwlHAGNxS4%2BELFBNbA2GntRZJh8ZxxHiwR4kyuvQnN4OzE%2BHZcIcsYTtGSwCIvk1z6n%2Fz6upuvPzEKsHQncI%2BwinFWNv0tdAH2sOUx6IOQkJzJpxhRhFc2PemyEy4iW7iDCpnBmF%2FEi09AIpyiA6yrcNJLgKvP2Ci8%2FjTx8Tult%2BAly9vYQW0JRh6ntLmmbd7KYXX3LCHMsGl7VLBd3XUhzJxj7st6O51f2f1RYSRmNfTw2XVoMkf2GAkr09jkLRSYQxGBJ4uWNamkNsC41NdPcB2ZgE8hQCdyoiGKr25kN8d2da4edOhlfFyc567D0rtmbo5XJD9eECL1GEeAnSh1Lgzchurj9dZBzknNcjPO9MK0kRhdd0AgwHCaZc0DM0BP%2BKoVczion4DrzkKgUdDuEcXeiAZKHJA%2BEizGe7t7cVdAjBF%2Ba3Urt5fVBVtskbJHeXT2xzY3%2BpQCTBa29I2kNfeGqEyi6vUavzHr6CcTTvpOZNg5WoQ2ObS4%2FwME653RBws04sT1QXTLAStXCb2%2BaedZ0fk%2BSVzOrVLzWytChC4vLA7mcCeWZUZxMxKAUni0SuL%2BRL9udeViEs8W6fCy02uLSaf5lfkehOrTZZkeHDL%2Fnw5JYnn3wMNPJ8ccGOqUB1z22EQZ9nMKHyuOfY7sZ8C4txL1%2F3sQWZROHBTx5Eego828W0bmR%2FLEp6z7tfVPDCBDEMVPXfiyq7ta6fyhziA%2FN7XLz7itEH6%2FxbZplMlfNctSHKSJyYAfmIdYYkJwnuaMISK8h0WCgoJpkLdqp0Qt9xtu%2FY%2FTNzLa6ookefz6%2BGUB4ASMOr7Hwbg4%2B2Gc7lHlb%2B9a8thZHcnXV8XiyEqXMnXF7&X-Amz-Signature=d2b4c1f1ab9feb15831f2dc6350db2e049219bf15e61ee420123d6961f1b60c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

