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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN5ST6Y3%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCZqO%2FjfmeP2YCYtbLbEqDmFV7XW5PoS%2FTzsHrxarF4wwIhAKGXiaX%2BVQrOcLe%2FBa0pKfpxxka4kxpVEelF2I5id0mFKv8DCFsQABoMNjM3NDIzMTgzODA1IgwjG%2BlEbkdj8079Jxoq3AMBDBbj9Z9IwbMew%2BhSnVJFnws5Mii6lFGpPA155dWdXoZ7z4JTvYf9mQmAIpZbwldA2vJg%2FWgE%2FTD9lmbol71YqVha351%2BPz%2BFhP3CwxbigrfWtgoIkQ1z4qgWA11Psim%2BnUpBhdttSPVcd60Ts7kZiDKzaY2WdlM9XoGhRK8INCW14JQtifyhigWI0MzUrZ%2Bk1Px2GAAb4s6THVviLk0ZeDzyq%2FM%2F4ATagskzl6KuLi%2BZWTV%2F2KnvIbKHK9w%2BcGZouPv%2BFw9lH9k8hc88GMUFusIegSmuLmfEnQkxdliXhYpecjY9ok3UNGjf0I03%2FnMRjTbYdchRLTorWxvmxnBFfAIWnoVgXMyq0eGO41uHp3tdgsjhnwHFQm%2B36Of73HDyNBdNg0njGYKbVI3uCon1x8rkIsn7zeK%2Fca5y1Uys2mo%2FK0Igd%2FM6xPXPzUw0Z%2B1i%2FKoypxnZeIxsfbOTC5HH8I2ogQbr4D3NDjjpNTRkPtCZ1UfOe3rBXYYQRGoy7QjdifwksnUc%2BmdbXmJ8jOmN%2BQcBvhqrJI32ym2AQPXu5KjegaNyTdLooubHgeFkiaBLWvktLQdfXcUq8V1W5KRzPJv7%2FZ9tdSRQ58%2FWlG%2Fs2l8rYer55QXuR7uvqjCVlNrIBjqkARfMrphGr%2F%2BCeYy409vuR%2BNFx41p5jwL6vX1pFZ3sNMEYe6%2F%2FuHnoEXYeDuOwUAXcogaQ2fX1XwCaU8xrbzT2bZ6LvhNYhxXHXL%2Bt4KSGRFp4ZaWfxfEJbgcGjX9MSrMkUAj%2FkOSDGCZRlSuRpNFOuLCEUHCGuH46Y%2FQ8J3tzSpGyAyA2suqdYnsRAHwtAlCydv56AsFqgjQeEKsZoU%2B5gMfIrh9&X-Amz-Signature=fcf3591e4c27e91bca3e25f6f7d710add40d31a1c0043d11dd3950f0207ed51b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

