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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OADEX5X%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQCkw96V5cg4YP1HqNzlCKyyrOttvDNqjOZe7LKalzimbgIhAKxV%2BKJ%2FMnz8tRzspIE5qxypQ6ah5uHFyF2yMe8mUlf0KogECM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2Fa8Z4qT8n5ufhgDAq3AM111Y%2FL2T%2BfJqjm9Moyjr1sLw891FPCaccvT%2BQCJ1FyMYwABF2fq%2FARhK%2BvPOqGnBckjTtVVMEk5wnumQPmv%2FjpFDQ3cTmxq9qaYRxzA3qVHX1EhQlgfUod2BethrTcxmgb7djiU0O%2BBTq8uu4tYZkZE2Sd0bo84KwHDJ16dOdd8sZQxhJVQWTAyUS0jXbyA9b3upCsatLBKm15s8sHBD3%2But4tIN93K9EBoUEBy1EZH0iKFX2Sx568PjB7TAL1oL3EVv2QpWX9KmNmzv%2BUwlg65vZwJvi9CVDzd03QD3Bxy6HSNTXtuFGiuxW3GPm6YCqzx%2BD%2BnEW3t3%2Bkr%2BiEeBLFsb0uP8zsGKc2gzk85yZi9D7jfyeuLwrYA6MLG5qOuplHhbjc8rkCogd38HxFqOpTWgoiXkaCniYcuBZ4P8XvEgQN7KUXukM8zUVz1lSND7xrOsFV31zYkmZXYMI5vQ2Lf%2FKyrnnUeuhYeUhvSqWh6a6F6ZDKvkEJ9Xw3fJzG%2FIMqJLEe19xtetgjWyEoz30rMaLw5uczyCX5ev%2B8jDluuCF%2FBBrYrCm92qQinVqtFgRn7FaJzgWDbOGMSD4d9QMy0rfaLXfrC5QwsEnwhhGzX1LrekvTDHv6%2F%2F%2BrzDSx7PGBjqkAeQmoj%2Fcrimbl8vW2YtLiDlN2JoikDUP3YPI5LR9ndaekCKmO73sEXKDIGAN137N2ib1J70C%2B7NFOy0IoaxII3UjDdC%2B6JtT5G4gruWglrKubpiwx0X5Rmpt07zjgfYWhVXb%2BNbxqmKNPxz8fefjP2B0DZ%2BT8yKMW6GsFl8mG7WlZj7m4jWAJvlfniePvQcWvo54wU5nLLQVTwNMiUvIKt00eaV5&X-Amz-Signature=2402019f69f72c79b94d3fb6abaaa82c570a1f5972adac388aacfb085c9c0b1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

