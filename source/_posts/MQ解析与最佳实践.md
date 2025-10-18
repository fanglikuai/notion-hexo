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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XC6RNV5F%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T090053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGdVUFRZh%2FusPUUF7QaTHVfOzB%2FDyNtR4%2BwVDaZZ0wo0AiAtEcdpcsXw%2F4D9D%2BFnJDihIvwMASoPHF7wqlK2B4gJdyqIBAi4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyFmrAcTaNEd%2BModkKtwDkr%2B0XK1HAlumwfvycvvxadThFX3yFo2V2UKIl3kvqdAy8z6yxWQVEUajCix7jETPwyECr2XMOQu5TL92JYI8ddsX3IPW185%2BHlbY9B6BegaGMGAfXR69nNM%2FohFNoZLPw7UGW1xlWfxl9K0Mu%2BFKegFuBosrXlNtFw%2By%2BN1JMBAOvzFQXLSrrEpBQGKreE3ngrYhUuEPGqnUozYrhz3GsnGF%2FMTQam8nFOjBD3FgCSvlnmYOMmZEAD9gUX4al12UyVDuidIjBKVxIWDdj3oN%2BcoYHM7zdHjS%2Bb0JE%2FuGxLEPdX%2Fw2Lkx43zhCKai9NURxbDFzxZHSAOWjedU8s0DGmNmR8XW%2BzV41hiURZAKobqVZAtDbnwxKbmiuHoPdzPZEzhPhGfFI%2FXFNGbQ6TUHVrcqdg8JmH8ArByo9Ra8kBgOHdm3W0sUDXHd%2FHw9lXx8KXgwjlnYttSAD9%2BbwnhZ91XO0AYqdpMivdWvQouxNEZz9nh9c6tunZZUtHtAHM%2FkhxPK0yNL0NIGXHXTGkKtzZlmPSz3BHN5bdNxABFvD5VfYdjOXbxew5IIp7RddfeUV9J3quRF1wI0rGtJ58nIlc2Y45bOuaBkFbk2xqEwBZ3qi%2B6gkghh9PeQHJAwpuXMxwY6pgGqnDW2diYvLKFlFj%2BzGloNFHHF3e3vxx9dqfKwO61cSrJer3ag4l5A5p2ahA3V1OlCnGrHNSZiUXqrYj%2FH7x01KaleYZFJf0EBLaOtgJeWa0O7qpyUqIRViJlt3raF2anQ4i7znulK%2Fgtin2AFunpEZTIEEglorhwrVP%2Fb%2BQZYthJfkA%2FcZReQnVb7XBahHXlG4i394Le54IxHkVvWCWcTIJVbdS5H&X-Amz-Signature=68287187e93acf0e1a7192747986318f303237c4aefc9e27bf6089e1760710fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

