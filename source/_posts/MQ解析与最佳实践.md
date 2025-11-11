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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPKUBOC7%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIDOhbZ67Lb6pWFNE0QosUJB%2FZM2WMRKNGxRmuhPlFZm4AiEAgQ2T0u16bhIaLYxxVlC09h79CKGeAvj%2B%2FZrCzU0ByiUq%2FwMIGBAAGgw2Mzc0MjMxODM4MDUiDL5veLwVXU%2BKoWes1CrcA4H1IksJJyzzGkN%2F86KvJFx44AfCSwCINocRHeosahLbfc3MlPiCLoBNxrT2mG0w7mVnPbfwDlUje%2F%2FSfbXXsrZoX9c10so6w5VJqTQ8NiYmWozIZGbro715E4pdcmpbR2gk4MDhxZ8Oyn%2FFBppRoBFklSbqn%2BQvsW3EwK2tezEeJehDYTy574zRpYB1IgvKiBCx4eL93jSmDdN4y6Wjv0QMMVUGYU3N7OEJ553vs5BR2R7NQhzeZBdNWcTaX01MN42cGRCEaBoYvfX4SvDZMskvsX3IJla7H%2BT6U0dhyMZZ2KMZ3qvo7VbGv2T6MGW6QPAI2ekvIeHGT3BSeCWL5abUh04D57%2BfAGFrwdkrq43v5ObyjFO8OP58zB0lbN2kO%2B0pTt7h7kL6AomvoA%2BbCMP5TemsPgRFst3gjm%2Fdz7NERohaH6f1p1oKK0yEB4hFuhs1J%2FXiR1p1g0sZoe3He3GgiIDvB7Ynf%2BCCfMzBGcRELrMsVHQWrFDWlHidNCO7c8jYQjMh8ZM0pTzmkPhq9UCJDLKMR7DqtH1w1hosjXtKshsWDOqSeXmK8Op2Cb%2BSJT4E%2F8i4JkB3hs%2B9HeQ1tuRQ4aqWXyyNb2oovajR0%2FuAwcy3BxH%2FvqWkOhyGMLXBy8gGOqUBUnHhZpl9GTa3YXF3CJyrnZhJVdw6LxjpGHzAdNdo2LMNex067mfnZaoQMiQ0Wi4Be7b4jgV1%2Fw06O0tAPCHArcu%2BUevKBCik9xmKfOx9hoiwgcNJUK1T2NQhfN2h6keNFz6fq0iJoyjAIgclkHqURk2sP83r%2BDAehJbyI6QijYTcfhSvJf0gFm672hhUeIEPanpNZ2MBnYJAe9WGaor4B8pVCtXB&X-Amz-Signature=fe9fc3b3228fa9f56e42fc9af0d0d75bd434ba2dc6190c4d04f1f4297cbce8de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

