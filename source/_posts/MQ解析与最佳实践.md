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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZSGLFYE%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T140054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJIMEYCIQDmwfW0d%2BjI3dTYXgtECfDlNX%2Fk5sLDlO%2BuqY4TC0WkmgIhAJ4IE3bnABWecfKKkyCc%2FzZRSKG%2BBpyWDYLjfltfTLW5Kv8DCBYQABoMNjM3NDIzMTgzODA1IgxObz8JREjfLsKGSDgq3AMHxTQukSlRLbdsXZUP645Z3v9vHnJw53tbf%2BUyVd9LGxvZEgqxe53NvrsjOElkTmaZYrN5W6RUonLEfYkU8lxz5qLJrAEVvX4hKzBGZGyH1xmRPUXybxc6yYy%2B7WFiA%2BkBNI4Epp%2B7jNgtgcI83vE6n386VUcBc8zI2kO3n%2Fa%2BnvfIm%2BBxdTpCiOfBCaqFN6UpfZEgEC73f7ZNxqCuImsKQUX24jDBmG%2F4WEZX07HTjvT2kIjEAOEGwEO3Lm%2FFD9xfGviw2xNNwwkgIlIZ1RcFt2tXAqVYU5klHARgIbnl6m03UzX6b4n4nd5Yc2uwb5a9o3tuVO%2BBoIgBTyR4QDrobd0N%2BSi4IIKhoRQRYugqW0NnkOFbpmGGhXJw8VItFuO9oI%2BgiFcfkES1b2vG16M%2FM%2BhX6CLCPLXtYcVJHO7ADt9o92Wjbqe9O4u7y%2F5sH08zq22BQawBwR7rsszwY5bmGkVn6Zwm03MM0UxcTlkoBG4Iiraf4SmDf1jCBMZV88NXcBBPuQgriNB8FUk1B63dJLuM1Ago7xZbxw6ppEpZnxiZX%2F0JroUI7jNNmPEH%2BBA1x2gtPRFWvebbnofynGfJ6E8t1S7YkCpTUlNt4i%2FdSHe0Z085Ok61%2FPNkHTCN%2BN3HBjqkASIBIHiMwwJ2Y%2Bq02MvCo210LtldMvDklP5nu%2BDD2kgelBXwmwS76KXr2btOzk3E3Syvc%2BRWGVmeiTpzaoVQAJqP8z5p7NkqJvMx2tbkuPL%2Bf%2FAKjIqgLeSE8UJuX8toZuPWBzvUazHTffvI0gFrww507uRKFXqXdZYiTqzIr08imzJDvHbo%2BzGLjvCgKHCCrQiAOn9AcZxCgR40G6OyG%2Bf8FPS9&X-Amz-Signature=2aa1b3e70c9c23e23ad70827281791ebe9a48df045abe7ae1a0fe49acabb25ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

