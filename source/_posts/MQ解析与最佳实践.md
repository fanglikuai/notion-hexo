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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZLBT3H7%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJGMEQCIBue0SdHbQEMhyrwxp6jbma8TrRZWMIIIhuVCJdRxscBAiBhUXqM%2F1yuDL%2B0K7RdPyrTzWkL0NdOQZDwJAWSWAIIuCqIBAjW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMV1BiyZVS85ts7aw6KtwDS39wm5jxVzH5F7DgTnSeOrLH4ITJ0uPjiFyZkW3aXTqY0PaV72XNNF9M2FEnQ6aJpZsimg44hJG0dPODVixNSW0TOHNGIoTA7ZmaQVbCLd6GwBjYfZpJr1HxfUxoNOjpS9LPL0sP8fAFGkSI8vm6vN9Am725LIke0cjNfXkgZPjRP8omcqvpMwW4dfCFCn704pbE%2BIwbjjYinNlnbPmTD2F%2BpZpHh3DIdWcBrDp4JeBKOmZ%2FEKrWBrbJSMNCPK4RxAiml5QheEJ8ny0EmsIeLt8OSca%2FMBRYz0KjpGP3PkjeqzRnL3V9EDiA22xj5VXQNCkimq4%2BVhmj5WqJcL8HStn1I9Lz37MkVAMYVSlIdHb5Db1vczLW6t3NPqnrdtj3md70%2Bj3au5bSl9z3ch4vqV%2BGvi5Gyj3J5lSmliBeoey9MtxPVBOpSnsWxlUvLAQFKDB7%2BLVSqv%2B8E88%2BpRCAigasEdEF6y%2Fs2GDbuxIP3kHxYrmMxMjOUmL76np7hqDahAw%2F9zGd9VoyDsODH4F8UMEI5OqFa0%2BRZzDQMG17gi0iLnfUhlQe%2B8P5%2BqaH7H49144r%2Fg5zkrOHWuTc2QEfTsoNGaWzl7nquR0zNPBW2kJxS27fNCDwjJk3JNcwvbTTxwY6pgG78vcEigbxd1cNDhgafh3%2BOXK4QmvQGLFNaimqrS9wovWmzJjBk7a%2F2WPyAevuqCaTGS442lA9edfa6AMerbvlHQ8u47kkFoELw0s7hCIPVq%2B45sEKl5E8u2tZJPut2IeRWFo3MKUHhiXf8EO7%2FtLUFEkN14UuAf0x4cI9f379d1PWApJnIA7Uzp5EVxGt0v4I%2BjYxbRK8ig8xPkiAyUYHOciKGEzz&X-Amz-Signature=e57287a72fc6cd57850ff53c4d8d0b2fb229bb93e098ac1ea67c7e6a7872b93b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

