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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMNYMYRL%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T160050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCICbDrVjXZF%2FfRpY1qVffhF2xxvD4yjyG9u%2FGQsnNCKPGAiAUyMuUjRBDAOuobZN%2BDvhgVMNw7%2BY6q4b6haEfBqLPmCqIBAjB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQdgqgy05j11xDHtaKtwD77NyLqb74mgo3c6Y9%2FP4w3iAUUo7gjWZ8I0ZiYoKWes6TLdcNKDsNbUArHYttfwfQcUe%2FJ6XZiLG2bx3LRX7HCUredWbZYUZXABGJtH%2FVxypqnud8PulFDXOLs8IIpLC1%2FLWYHQMV5DBuPjbVkRRdfdqbqTRZGA%2FgVUNscnJDrm1CfZStHnR7N8MjJ1Yu16EgK1hwm7xF9kOT%2Fq4UA1gkyzaxU25TOkY5F1Ih6sP39j1uzHh2nZp5lJgik7RAHgECB4JgItlOorpFR5zK6vNKsuPWGOZoMqDzeJoN9pBTUkLHG%2Bc7JHFKol1Wh1YgdJ5RsSwxKR144MBZf2S5nhzovcUVX5U7PIcSWhoFgqYbsywyeXqnD9sTDyOch4Vk7LkMcoWuHNynb97%2FgE8MhghEeGmEm%2F0YSLEsjZUz2XAnsrvBpkTf3fwRafAU5uPaIGP0Kl%2Bw5v9mjs04aP%2BOzOlIpQxaaQvJHceU5cRfW1yi11A9%2BPFtGlofSnp0oeQmbptCy%2Ft%2FKw6o8CN1SxAkWx7ksgp7d3uWmgelaY90pIXQztC455ya8B3ol1wIZknDZVZcXBb1gyS3M19RSUPqIFGBW21B%2FBhepdfWh1UMB7BvPDlGKck%2FY0MbYS7ynww48%2BDyAY6pgEALf3WhNdzvALJxt3Cd%2FAg26jVb5L651Lc%2FB8Xqo3t3bKqRF0E1DfLf53DyzR15SroMulwI%2FhmV6o2qosfJ4qh8fFmqgw44Zgk791hmo92N3mxlkvZX3mwZ0R2BqhcLXIZ7yY1Te6F6DpBae6Wp5r%2BrqtO8VX9RqNnQoSpTVKdfXL9djYMozTIo1EeGwPV1su%2F%2B6oizL%2F5XMary28T6hxyBvj919cW&X-Amz-Signature=40971bf5b55460db8e1b0fb3582078caa0cf9a5d0a931e774f964f47260fa7ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

