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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VXBMKJCB%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNI8mN4ZIpa44eyl6kWRdfDRTqzBo%2BNZ4BJmoXXiAreQIhANMs4lkbzFcPela10p9WmE7oi%2FDDQq%2B4gNfOxuAg8jhZKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz07uZ0A7orINFx5NIq3AMUT%2Bf0dLRYASCnw4ebzzb%2B%2FTw9TnP9h6SJu5Evj4XhL2Xmnu0jyI0U61xutQuXnlIJnsjRE6JZDGu%2Ber4%2Bi0YIbT5eyyFD5cl9gik71qjGZLpOTuFeASjFdMFcWGYjSOkE7MvRZQkXWI1W1DJl2HtjJg1kr2U5UduHqh8ejzZOjbIr9wJSLBogRhmcck2TZh6Ps67qQzE6WI4%2Bo5Ppwdfupj2Migc%2F7ZtnB3XG84L3dOUZQ730BZqEyzbdfw7VRIsAnlMiR2E2j3cCwMq9UNRG3pxtHePlVIKRrQJB62bkoeVGmS8cuc26UncJdRjbS5fT7aUYxaB7F3j3bEs%2BFDSxE0WDBQADUMDJBF%2BO%2Bj5dNTdEiWjhNI9Fob6mVjoVnhZGjemX2XUJLmC%2Bwun%2FZI5ldsUr1Pz%2BBkCoNHSojePeA8p7A5oYA%2BxIMfZnPa5YL2%2FAwVFFIyEhXpvw9S88V1ZF%2FdPUGSWW0HopHkbApkvirNvA2bIlYwzIPvjdO4L016Eca1cIEX3UEpBfCbIumg20H0D%2F%2FVzaSpa9ifK5EL49OfZcQAyoDOQDvczLBXUuX2vizc3RXWmD%2BW198Y7uhF9BFWHnqt6G9QdsGsmFho5a5IL9Q0%2B%2BYdWo8S3kcTDSup7JBjqkAVEN9PKLpQGueeOwBNsvqD1uecTJNQheq8seTfXohUwN5D1NCEztC3fo9FGuK2rVg2efoZjLdAkGU%2FhpjKEUsLrTKnV5%2B0pudh0zgD5%2FLmymgu9CXDgG5yimvgXU0RfvC8mm7n1H39H%2Bk9k7qezg84%2F5%2B%2BcnxEOyoGfUwr83RuaJjjeOAKYXfkJx8BIYskI86YmHVIOjRCBKtrxyFyUKOuTzGc9y&X-Amz-Signature=a9773aa9ba39e4c4b3c9dc95e811cc2b7cc5ae3e6fb88a8a654177da8fe0309d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

