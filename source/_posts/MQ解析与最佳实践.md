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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RFSN5QMA%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFLvYiXImkR4fzrMyevS9xuhbzWYE9JQycc0Tew7GVXWAiEAkvuVApatXs16yLhioiC0MlHnR%2FfzeJyxBxXcwMojYicq%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDKa6wqZ0AVXt%2BYygrCrcAyKRIrMHV1oOfKyDFIEmsMwgd2tXO8V2vXgbTIMIV5%2FMcIq0%2BA8GouiaQPGfyqy7ZdXqFQS6%2BAaGgLgHtjHmbRmgOIGjUWt%2F%2Bfft8aNl86zRbC8zU4acHXBrkC1XBOHOI%2Bp5S2BCzE%2B8Gw9qHxCiGdoNKeYTiZrrjB%2BpyEntwZuoJ4B%2BmcaCG6Unjy3%2B4O%2FKo9BAgdRj%2F9F5JUJAK7vlsJ1sdAPJ3I2tXw%2F3sgwQ9mWJhRieZqzDGVLADhfP6jtEKithfTIKwYbzGsLY8hBY7f1LuhrXZ0cSn09S7sSz9VvDQfUlT4hTB5vXbvMrBxTJsMPLIsYnecTPuRXEm4IhhJa041eVrXWTq6FpRfpmLLW0pYY2aDdP7Aa5jRL9ruvMYx2OH2WAdHbv%2BoynTIsYS0%2B3HAeLdm4CagrsMVNtO17Yz0KIyE7tgh8CIxEk%2Fjx%2FJ22ZeQ1NqjEes8Ed0rqvSjkR2Lf5iIH9ltEyNgoTT2cOYj1FuCcMFVreF4CkjcrJ6lyiPfrTsCPeRwci0h2Hql36aFB2x7sAwtlX%2BR9c4VsTeyxN7cZR45xKMlKjL%2FCdGzG4U1wzGVf1UX9giVtsvL%2FMQ4WiVAUhtsumn3Q%2B7o7CViVdFWMJwiwepvC0MJa3kskGOqUBdU8ww9ZMyASXf0mVnn%2BO5pUpnmwJhhSBV03QwLeVhxMFGCVwTJbwFsyshTJsiTxT%2FItTaUnSZxFXnh9TQB8B%2FcHerm2XyCtdf%2BwrIQeKgpyFUHfHU30HeEYwKfS%2BHjCc1lJtp%2BIL5iKF0pP%2F6bRhcHA55jGZg2AE2v9wUNusFZO30i7WUnr0mh0Oyu9Salmv9o39dMy7cQckDhkFp7jrVXsOJiG7&X-Amz-Signature=7e0e1d94bfb90e93b539fbe924e8b527661eeeee392c49a80f6d19b322e19d32&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

