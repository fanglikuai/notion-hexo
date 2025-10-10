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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WIGTJBY%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJIMEYCIQCJH8xTjt%2FR0DqKW1iBOH6f8p4VrGDXiSBor0tr2bUOpQIhAObNCB3fZkPA4PcAAwVJdpABGFIBPTVUNvwZ%2FQDN0qyRKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzVcpxkMXJeWnuImT8q3APTtZF3wfNbfyJsezc9Z5JYORXrmA2RL%2BJxEQtjj20DfRa%2Bu%2BvSxV29WCFVt%2F8uaG1PP5bTk8d8Yt8wTUwwaK6Vdc8IU8T5wJYG7NimUdnA7a%2F75b1MSjfF5syneh9zsNkO4OQDy0XXGbAxeH%2FfTF29rxEMxrgMhPx8nhsjC76lpGFXzts6ZUcP2x%2FkpMpcbqlv%2FcbuZ%2BdW%2BwuxdexIMNTG39S0ugTg3bdjB2HQM8KUDUqoDgZgHJhamTXtnMvdh2nd%2BFNr7pE2MKDmCi6L1jIraTEP00DW7oHPP7wi%2F14Ul88y3Xpq7HCTj1cb7nGD%2FLWcNGjDoypngkwFY1JDaMvuexEDKpELkhZhOP9%2B0do2a7XtQXyl7oI%2FVTajNXqcr5pr4d%2BWn%2BE%2FKsy5L%2Bl5o%2BWtY7AaYGCzWKw%2FyIU%2B9GA6JCxs9jb7gf%2BbCJVj1CLJ2o4owFYVTgQVKKGQkphS3vxC877MuYaU%2FROd3sK8r8zI9egQMK%2F85z3YVanqOu4ZVfVrG0bAvDvrr%2FiRoYj676hSg2id5CxWwV1I8OOORxkG0IQv%2FY11qT8%2BIuE%2Br5hOjT0IFROU0tW3riagGIf%2BDA0%2FIdKwqkXpRxFDl0TJM8vQrYrIFLiql0trJ%2F2LDTDf36XHBjqkAWhEGmuTkX2jYsKqGyrFC3dW135uC5LfJ9%2FcGBU2Lm8QXykk5tPCrOC2BRnL5umT2hN%2BUt7GCGM4lD5q9kLY1A9jAVVICkRMpHyYh%2FLaaS1JJBrSx5T%2BQAA8rWZL8%2FRFkYRrLdTvHz1Knljn%2FS07Uz1yPhtQ66FjSJQBocSswILsubuiifkpFMlFxlycAfpf40iWshBh%2By7e2NeEaR19%2BMfoNVDe&X-Amz-Signature=c0f1e3ee243b08828ef6630172053004b45a6dd2bd0aba2d98f6eb430a58a5b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

