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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZERJOFLO%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJIMEYCIQDzz5Vye8ywz49iOT0TMXb6N0kswqA3IdpvZzII3u6cnwIhALKaUYxrhe9sLhtLBtIkWY%2B%2BuHPn9VaYV7%2FOm8rXSumDKv8DCDsQABoMNjM3NDIzMTgzODA1IgwcPhiH3WUg84KxGbQq3AMjdwGCHOlWHMLuEayzXVp6bY%2BdRJaq1%2Bk7XYmXZoguCYMmMX4tneP6CI795NQnn2U6wZb7lxKbrX6MtbcYqyl8jBeR7FO9O6ch2sYiar6f9cWw1WhOMFlknZerEj4Dy9YDHZ48du4xt3ktzN1qqA9IxrfPQ1hZEPJDa1j8%2FtyLe9ZanOK4%2FSbWkFPGV7Sb%2B%2FpE9HPQB%2B%2BCNZ2TfEw2mdYBBV9ZECBWwx5971HJR8rqSDIemH5ufV5Rt6tiNByHrGSEqB8sv%2FwOd%2F0%2FhzMdkvHAY8vhBm08uoe3gyLsYyoUuc4OiGdxPiWAzw%2FFDaG4xgEjLPabEwlbUb58%2BmUTkjgzGfdj06LIkX38pVFuLHMCj6WMTDmnUzvbz6yJOFQ7q5uws%2FLlkZRvitkG8wE10DgYYUHmAwxX2azQOxsZvpdOmu4UddQ4fFqid99s%2FxP5KPDuDiz4Rotie5vy%2Bgye9H%2BU1%2B2N9Hk%2B7%2Fm4rIfB753X7dleCLSneIIj5dxroguVMJoEQH5aR4559CHEOu713xJtwEU4E6KNmuDlqvIU7Y4Z5zwZSAVfHiPi6bXQdje%2BogVtg5xtMTO4JPE8kHMqvAyQ5ltEUv5iy15lDUTOjyrPyE99JYd0PTQ98VchGDDG8JrIBjqkAQ5QiDS%2FOOliOn5pPh5H4bqmB6QATfNRfBI0iPSQXLBQfNdVizq3ilvZvZdBkBbzwGpbe4zJ931c1LtVltRVmihvxYMurYFLwzB5mQEEEr3XwWcBSpO%2BEWsNJ2O5saHDbTUGF%2BNmfQbuYWLkstllr8HX2wWjZFQ9THkiFfYyxtKLW9KkbVa2hE5HnVAq97PPbB8vTo5NJrIU8rKicjMpdZCruqsL&X-Amz-Signature=e2102f414b36b90cda69802d8967b10c5829628416c53c0d8398674642304c76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

