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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZUQNRA6A%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIHsBZI%2FdWcLVmm4sDbrASprF03DDJj3gv%2FpY7Jd16rxjAiAk57qt3nlBspL9GkGgzZnkiuD2ZSAqn3l03E%2BEb9Ib7SqIBAii%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMb7tnqysV4Lar%2BOKqKtwDLtjY5ufDAMFD6PVDLG6pZAIc%2B2UqC4Rmo9%2Fu9ayZDl22bfz4VdBo6KfX2BqxqzSH8x59XhIH7GNWnsO4VV%2FTLpbB2whKMxdmdZB3UHh3g3oydcEw0EDE3o8JO4vkKQkOcrP4qIGGqgXA79xL2204zHqWtX16aNlOY8iADWg94MetcCdAd33VRCYg1AZDY7oHy0fbik7Sws96mIUegQAtlpPvZ5zKKKuud2KlUUZamJMAQ5vtax5ZkpvEhj5IqJHdU8ByuOtDbCT%2BhkUH2wCytarQA%2FohWxhwQ0EVlMb3kJ4dBLo5zVvtQwjBHX8hPB0ovHow9Wg4gpWnIs40ZXtozQtyuNAWaIcIWyzLebbdaoNu9LWywXh73CL974wXf1vGDaQ7EtCPzTZsWZJmdU%2BOGw7Vv0kq%2BQJXJJ1DSVK%2F5JlazZwhaJ9yrfpLIW70N6i7IywrHvbdPyVWGF01MljY4G8L8fnrSVrRujZeHTz9Io76FtQFVFJeGgtt5Ga4ua%2F5pf5vNRPv57Qtlo8ypw0Jh57rkxQW9W7MNxB393qhmvh3h0XQ2kZY75EwBEYgedo4dN1OsX46odgy7O6sAVncVoUY0Plo0CjC9RWcITK6fwg8BnwHnUh1jvCONAIw2empxgY6pgEH3bwGL81i6n9233sC4NxXp2eHmdplnBp5mrGYWxs7wEkxiZURfVUhaceDc4cpY0HcHzmH024kUjFycVadAQ7tLdWblMqxtiat2b1hiTEUoHu19Jd2vvDgc00xJeiDrN4%2ByheeT8rODZ0sGWv1UkaEkqYelCvlcnFmA6dWfDtLI2vnoC3ROh0QaE4N5Ah%2BQ4sO00mKgv3QcCPotwuQ3uSEKYTTaR9E&X-Amz-Signature=bc06d95e0a0fdfbb112899f0cee732641471f699a71423096a3386d6c6d3dbb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

