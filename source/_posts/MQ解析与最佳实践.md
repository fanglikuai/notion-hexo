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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXOSBEBI%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQDFFPuurj0hy66zeIlOyysBTraVLvcvJxjNDsPSQEwe9QIhAJLuimNqyeLzZU5vTFoGfRvWIa4%2FXb68pH8EZk3CR3whKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzlJc7NNBHmCgMekuEq3AM%2FXpe5J%2BpmcuQxpbJ50aP1yYqa5QvPuQskzByiAVjGRMV6baHdsqsgl0VV0Kc92vRczPhc8Hepcq1%2F3RQSN2NODJlf3cWqsZKQwdQ62V9QV%2Foxy1gCkwEFy%2FPMNj%2B0AZzz4Tnr%2BVp4ngcifLvb1mhufJ4PoKV8yHqLRpIDAS6FOsNfHrKz1gB0oZn3gK%2F%2FguzbHffzmRsbV8MfZ%2FqQyA7HLmI4S8w1cd1pPV%2FHhDSmNks0rSRm6NGj4XBv%2FvEbeVgCAhsPsj%2FM5d7XWU90f12jYboAo3Kdm1BfTW4CFP6uP6YT4SpWABsaMJ7xxmmydHReK0TE8gEngwMeuM%2BObNM5L59FbEPIthpNJ7KH7DDvS1fPDMhwjiiD%2Bu8QA73qSxNiJfrNxxw9fAXRJCucZsI%2B2hJIyYq5unxQSH%2FKECLi5RhcJD7FM18VBbrXXpKllvtzXskeeKax9YyhohUTKzMBkEiBGvLJYq5mh15tfuOdRcLzgQB9s9mSF6QJ7sAe6%2BDG%2BPQWPG%2FxNMNurvZQHBVR%2FM%2FhOAYwC0XfJx336Hpp9Y7jKtFJpv%2FSyfxAm%2BxCM%2BGaA56th02bepCnAsibnszKRCHbavrl1cknjWvv%2FfOEERkmka5ZqJ7guwvZdDCzquHGBjqkAU4lLxJ0RSg7N%2BS8UNrj1sKXIn4KcUYza9W5uCq59TeVHV%2FnjqfTc4%2BKdugorc8igb2zrlXXEU2dS9v%2Bjvn9zRZxmAuVuYh0mVcKZv7KDelmXeKP9TlErgohK7ejUacESx2fHVcAuKx1a%2BYX8%2FYu%2F1E7gjxbPCEx6ZLp4vgop2u78yottodA0Qw5Xik0GhY7NJXleHFlQShndQ2K4itMUpYK9I7T&X-Amz-Signature=0a900da541f8d199ef138a84d20234e426441ddcc2850c099ba10af45ea1ceb0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

