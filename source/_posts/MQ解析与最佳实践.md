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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJTWVJTW%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T030059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvXaPglCbCqrsmUvlHqe7uZVQ4B54bsBj6aHVqIrkEhgIgd4G2R1JzOFU0NEMx1R2miCJ1QoeZ3iRiBJ344gR42sQq%2FwMIbBAAGgw2Mzc0MjMxODM4MDUiDHIMq0ZxD1QU0o8MyircA4T5B30cQ4u8bp8FnLAvlsx6Y8OdY5evLGh4jlxRVlelDSidWCf3HFWGVw60P3HejrVhgRUtVWJ39RTK2vxpBIHqgppV%2BrLtmd58zhBUnBwFoNt%2BPvZ0xHMk8RNwnj4Q6CyI7zzw9LXnBl94HgaaP11eddBlkM2gTvc%2Bf9%2BTBnCJs2CfzRqapyMmh3pPe0U2nUTxnLrB4vxghyGRQbwjSqa6ZZMltpRpfgxd1Y5y5xIc7sr4CKTWHaT9KUc88gaDVnDVDfH8G1LzM4xVq7wS2NPiGsQwZzAT0O7EdK7Ngm%2F0Y4Ecmk%2BBkUElDYkfp8Hl7Y%2F7J%2BepxtLh9BpilnVixya1RTVFRL%2BjK0Gt7EKmnsOKZrNhQ2nfeDcOJ1vwKksfNuFpkDa88KX1img7jTZcoV4GWSnQHcRlbpmCbwPE7paVvRiubu7JI5mOQNFsqp3gXwLeWRd8C5N35gWdjNmrzfmbewlkAhLeStYy3qR278EdtJ4V6vl%2BjpEIociM9KRICxMo0%2Fd%2Bs6IN8CLs4Sado%2Fis%2FqHR8POOVDv1eh9yfoybTo004B%2FGXyciU%2B4r0oT4w2niNBNx48OTH3%2BZfCG8qnNX34Asbe%2B1HsDoxrG4F%2ByKWd86hFbr4h29hLANMIvu8McGOqUB8LL8E29b7yJaQ33m12f221Dm5BasJhmNIB3zrc2AGQIRh8FTbGo9kZYX0yfLC4x90Qdc0AXIhTzLUrGBs8%2BA59BHM3%2BwSL0uVTs1sTSNKPuCWmj3YHberI%2FJN3ycXLUxeJcBPu%2BOQ2BLXn3ZMf1hzeUDfh9dPcvN%2BtuzA7EvqwuLOiTexNj1EZ%2BT%2B4MnGaiNotWXr%2FSgCAj1onuIJHiZWewB0fss&X-Amz-Signature=2e8050d84433a021b0a1a85e00cf8d41c95a0c42b690384b589263605fae144f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

