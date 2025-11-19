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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KV3HOKX%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIH3QdDxT6Y8u782F0Z4%2BtEo3R8ECwkQ9drSwx5mp9tvLAiA5Hz6hc%2BUJzg%2BmmuIFlIvUrxLKgEz1nIcLnFBfCFpM5CqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP7oj77JTNHFqALu7KtwDl9qIRux2BGR2TsTKVS2prR8ZjHJEDszhS05V4q0MYDPznRiD5DBPsEWkyoxJ7feQt5niMqHuBya%2FESplQsUEVQP6w6DdnmB2F7cNCQzQwKHrdlBdFyFJyfqhnUfVTriGTsA9vZ4s9WQCawgbHSA7LzpkluXmC3x%2BsOG9lnCmtkTx%2FXNmkBapCsrjwF7dvN0BuXzWTdiiUQ6AmlKxc3571JV4iyXR7S7F3bvfs4K%2Fe81zdDfWdXHjYB15Ny7Sk851CyyYuXpVqKZgnrI%2FVvJ2MS1RYjnLL9T1ahCoxaZPv2oX1vphdboowFo0B55T2nWZKPvfFPx7t6L5j1MVA3jO1abJWPY9%2FFkEKh4KGDUbFFtxk63g2atLHQjItnkm1WXa7uDJ3MBeVCTT5Fg7rNXs4L%2FcnWMzrvUg6Ho8mXQ1eyY9qyGEP3c2d70NV4WXJaQx95Di%2BCqpIEvmZu6St4trQ%2Fq7r2fiykq9CMmtZrY20rWYwo3sIt6cjZpiYb3SJD8PUqv5s4zqyORPaE%2BsPSyvV0oHcTicGg42YC4ChPuf9VLF9QBIkpwISAFpJS6%2BChSdPDZAt2MT9y%2FNcTd%2F%2BWIhHqGC8DhzoerxK%2FO7bp1iEpBi8Eh88T6wIPrvoCUw79T0yAY6pgHmhKzap%2FNpBMJAvjeWP%2Besldti%2B7cvvyosZKP%2BJDScdHwIsVDXXJOCdDoBefKo9n2WvCqeTUCBfzi4yVYDK7MaFx0HWPCLLdM06MrXFJljXbepWFMXdrlmuLj5x0MNgw9jyRn29H3kQIyx1D3yFxC%2FHoHVFDMEz7ptGmowLPnwKRW3JtRVS7A%2F9AKkt11bZlDsmgKyoYFdYPzYYzOmwvWPck9OlO7%2F&X-Amz-Signature=4e97389467fafdf8d80fc042c3376f0f5ce5c36a03dda3376176d9d5b2bc4156&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

