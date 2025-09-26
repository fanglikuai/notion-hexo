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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466775QHLDE%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJIMEYCIQDJ9Eu4LcHqoTO%2FXmQmz0hL7f%2BxfhxvJ%2BInNAy8AAsrGAIhAK5BRP7zysAPiBRLQgUqqIcnQolQ6CWAo3UJIGBdiE%2BqKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxEhubeo53Psev6rgMq3APmBh0qbnXON59gBF9Hy1IC%2BaI7jDbxXQmMiVlHzkH6Voj8XBEHvD%2FpZhEgEaL%2BubeCeDaGZfS9dp78X4erF34m0VNDVCC1zdvWaJugqpog8Caaxns0fAEiFJJBo%2FzVLcInndj2CmZsL0pgqzBsf6rkh%2F0J3PSid90SJtFYNqEIyAziSEtRTDdLuYBZrOBkNMNprI2rwAC4H0WA183wI5Ut4F9soVDxshUV9fXoXjtITpgFI3DfGRW6%2Bl39aKfFNoobd5%2BfpmZynG7i7ZqOPCFQGzmH4P%2BopliT1QkcnaQEejYosDpjEKWQeKlzvSdi047SnYnmV8vfej4AAI2yXd9N9i%2FqQCQoGQ8npUqZRzvnwfT5V3Dc7YQkAu7%2FbT4v%2Fvt6%2FwIZBLtpyIaYsPQks69xORILGEvvxwlocTxGQ52t19EXy9iTCXY1oD6mMPJHHYs1fkAqtz3zEQl%2FnicGw3dHhww0S3dp9VEdirteduGDXe%2FcgyqELzoF87FyQjMEI2LBPBzi8Ylum5wA5wRApSFdtwB75gBJ4VM9npuYugHy%2FZ4F%2BXWDftU2vjiKB20Al2NLsTLIX3K5Uz9XyGh0R6CtQ4V2ycC8cdyyjy1KDQUisIw1rnK6nXfKMfXqwzCuvNvGBjqkASc7V873zFwpLFDuhA1TMFnUPWB4YUzCTso1d9oeYMEZ0wfibax4yHf8qIpBDftJZCcHLtkrtfAmyq7LVaQN5ewubixDqsin4jVa2q0kVytImKIJrkiXiQhL9MC9rI%2F0KVmeEF5%2BwG1WoFKeC6oOIaEkusR3i%2BBWjPKi5S4HADCJRDNmxp4hzO7%2BslBmDBD7BKBAo5ef45Os3Fv9jHviIwz20d%2B4&X-Amz-Signature=3e8be9b996ed81074e6572ad9963813623c6561f27def1f6f5aeda9a7e894261&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

