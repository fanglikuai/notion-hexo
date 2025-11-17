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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q6OTXMAJ%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICadG%2F608L3KbG18FDEkYam4Mo8k%2Fq8Hbrj1btAj1CCvAiAoU51IlpNaN00y5lZ%2BLDFxif5iUn30Hs8bS7xAwrU1RCqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPZ9kV0GCKVAhyOKCKtwD4v9dFCcwS%2FbDVRyQ%2B%2FJKlhl7T3em9khMACSk8gYRvTqpJAXJC245SnFgrXYB%2B8gmIACkyJ6hnOVvuOmoT7jjdMXkTh5dJIO87rC1vPtKpmSqd3tk%2BDjyB6x4FULARvsDGPV5CagRnx4Vsu4qwTqIzscYl08Xd9mm7BtpFIqDiCiMUg%2Ft%2Fcgs52GRe%2FOfEZwqQ2HVGez4SSb7z8Z1jYhJpFZWKV%2BsK8ZbMP0hjYG42Shj3zh5cp%2F5fBDwA0qRK%2FVKfonXccUerG%2BbU48x6ihedGyykEyw4Do010gO0zOUR8u9a%2FlzEhtvoAz%2B2QgttLIG1G3NsBCBpJqwC0ZZ9qSA57UWJbvZBJD1H1lqj9yB4um8um%2B5PHES0inxnpGn%2F3pTcQOuobLq0GzzLRZ3%2FfLUlUz01rXjy9%2F6Wx9ItDojI%2Fm9XwiEckwaHiMwLDYdlkUyIMS43kbrXm5hsEcBiWmzCFgqG%2B%2ByvIwOCMHlt5nnitPrEOy87c%2F9B67QJF5ajMBUXYJsIbRQA7M9%2FaB2MDtQnGOE6sYnFQ35evZtBMOl91V5ptEL9gf50Izo3HAxPmp7RhmpDX9vqNElAfATuMGiX%2FOfsOgT1mRWuqzzQT6rmcsVG3yMpYdxbrPyyF0w58XtyAY6pgF2beD5eI3gOeL10N8ALzC7evqaCi4eNfxiKyjqz6qyELbBCxbnfrUPBK45qKRmees5kj33L%2F2ZSZ4euQSRR86ONANQNtzibm%2BBzRodDShN5SJYu%2FZoBTIkbVmtz2%2BY4BOICBW2q0KCYEud4uPXxN8LS3Jv2e2S3hWBtO8jROorevseP9rtNXu9kl7uSgQ0naJnkR0%2FM5IavYIXBODK9QLj%2FRPOh3TY&X-Amz-Signature=2a552d83dc93931cf3416ed220dd42ad28d1405ad09484418b1eac8e01c8ae0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

