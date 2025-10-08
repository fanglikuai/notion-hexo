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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JGUBSOF%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQCwBQUSHlKKgoyV0ZQ%2BiNa7%2BSmIg5I%2BmAhJ%2Fon4%2B3ywwwIgHO1OAZwhttrX3X2TAKIlw5rdS7QhMLv2QMlK%2BGwagXkqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIzTuZz1w5Rb8Zw4KCrcA8GM4VOy69psHKNn%2FwDxJxi7CcUxmPu2rVJHtHmEoi6ORRp%2F33ngBgHJb%2F%2BTp391fCx3YtdBufvUyqZoerED2pNCthnBLH8pnOwTe3seQg%2F0F4gJpHI%2FdjD7uY%2BrQqtWhnBNjXb5FnUMRQ7oaTVx85kUrdHVSUQzmRiWGX6uMHDw7gH01lZYoQ18%2BpHlEwl88APFsXPbjq9vg27P5lUeMeAnOPBb4qeZyi18tS4R5Qg0TWN6tuDI4wmNOUYPT2t%2F%2Fq8Pgj4PONxcWqygqzeJG5XEbxSiZfoIva%2BUiyTHsOmScVe2pSCECFr7H9YljF8uzc2mSd1paY6EvySYEaE1%2FWUFe4IkPdcgCyXAK%2FL4BFAjwZTJh3BD1u5WBaX9bKnx1n2J60AbNfU604W%2FONNyfpRIapOW%2BbzxKuVRJtAMIkbncfFODyZVdhsqf%2BDsesoohjlZTlTjVghlQu9I3osQaj4h5nePNQurGITSqv%2FGbhZBcmF1EYL4SNb%2FoI0l20L4K96TElDJPS8eSEhYSY0YoTASx4o9v%2FdowZFdsY4uHOYy7rLbPmWw13pR1cFOktLK2d5i8XdcURwJqUr7mZOupUecAEGAvoFQT3gu5JTlPdVbs75V06gMeCDpeu0NMMDwlscGOqUBglx%2FBTRh4V7N%2F202U6ge0trV29igy%2Bvd4x8x8OJ0t%2F3vaJQRXrBPArSXzqk%2B5N4lIX0Ll9O1AbYITV999jrEoqaU4qgyuBvQGwKV%2BrH1wN7w5Kv8PQCHNQ2Z2PmMtg1QDQq8UQkbHFWUAlBKrHcSrtGhlvEKrkCe7KNpWo3t37PPblZyWEo9NCT%2F1aGuJ9joW8F2uvgv0JEkVJgCZG6xp%2BKsy3uB&X-Amz-Signature=8eb189f4a4762d9095445f98b0973020f6feb29547a35bcb26a02f32caaa5180&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

