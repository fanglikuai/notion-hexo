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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMD3VSCC%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCicdwijO12OUC0rEB4tT1Et7aJN7pv5z83OBOlW7eO6QIhAIgG1wV9s5oaCrbz4OCOU6gwyYFhBh2lWfCmIDVcf9zyKv8DCHAQABoMNjM3NDIzMTgzODA1IgyTWgHYwj1Ep5HXFJIq3AMuBK6Whv5xlS8aL3nqxePbppettX4CL9u8Req5XTvaw6YUBsiVbZ9U%2Bcc24TFywB338HKaJfpM2yR5Aykee0DbFutbsVJG3eVywZcXwQtGlEVyl1uJ8rGBwRaLOZ9Kv%2FXbaSPKpgXKLHTcAxWrFJPT9lXL5o5Rhuiv0jhlhTMGYozfnfeVFXcoDffWpsAPfGsLqzRh7LGj5fPN7I3XC4xmIZ4e%2BrtHLvflIYzL1uoNq0qUzoS8Q75SxACbIv5F79DMrusVDnK5mDtFJL%2Fi8%2FbM%2BqVUueYAclgJi25xUjrFXeKHYnZEBM8IwgUU5tBpU4EXbFdbp%2BxifEB%2BzNsnmgxwDF9VLILtDBoziKlmdbR45Po5fNfmtF3E9iuRPzqO1Zu8rXs56%2BN%2BLGscDktjaGFlqg0UOBo2MUXwa%2FC9PcZP92iWC7kNpfH7iuayXKp%2FTotRqDnHTw5ZgtwTLcnaQvXiabxjIrluSWO7UUGH5Ok9iHB1FKwF7U6T4BeAgEUIlhJFV2Nmd9Datt2g6XQKX74AioKrvbrdlELVCSPwxfrn6A%2Fnv8Zf3rpSp4Ge%2Bj7LWoCb7OrwOb6%2FpiE%2FruWVl7jhgbOShcdaBPu1C2qIpTYANMjwY%2F%2FEjxXkedRIRzDMy6bIBjqkAXdwejH5m%2FHkRtsjil3kuK3mmgIcPRnqMlGUvxlnDgrCvMmM8OqesihVUZqGBrmyBiyR1FKE%2FuXR%2FEAfvncifKC%2FBTnwrYJIAI5Y1O1NISDW%2BKqu7kEpz583yPpTgNbNJW3C5g9fPYuu4HoSnM%2BA%2FKf2d5SwCKypiirAr7AKzI5xbiDN4O5Vm%2FegvKGztH%2FH6jvyt7a2gWtywfANWJQY9CADEsU8&X-Amz-Signature=f15538a3645df06e002f792b5fbedf5680310d7c3a32de68677637b6e1e92f6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

