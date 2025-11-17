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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CVWUXG2%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T090039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBDy0jOf3nY7u2ZG%2Bs8XUxMPFpMD8L5QQauYBo5lUW8bAiBhzL2yRvVou%2FGRB0e5RgFUNMlJONKjgVkKmfGfbYcB9iqIBAip%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM4IyensJO%2FVkN%2FeWwKtwDIAmMe%2BAUEYToJAOtljAnAFCSCWyBqes3kHjZsmzCi0HvVrvntKZg7iQn7pIi0m4y4L0mEW2NNiUfYBdIYsjAxpKTYzF5VN6ncg3tMZDfSqofN0IB%2BpZHnTNBmhqABJEAH90RqPk8OEq6aLg7IgOl2aKgM2k%2FdQZxwBtn1j8SU88or82U7maYxdkZ%2BoAjXJrmZwPsZDoTDA5v0%2BUZc6IeK8OxbE3pLN8z7Xd7kiHckkkZHH%2FnT5LUgrZhyXP%2FeWYwQjWOFcRSX3t5lgzTENYBjW%2FKLwl%2BzpTr%2FREuPvIOyIdtuGantGOUr68OmuYiaeVmpTeBo8%2FuQwRfgxoK6PwXvOz8LSzWq4YR6KwUBZ3jz6arqbCD08g6NboFMAHNSZnVnoNrqtVYNxgYm8JC98cPQ5OcSQz94CL5d3yOm032cggQeC34JSbB0thVJQrUoFeITWAl%2B4Adgg98sFOXJpg8qSF4X05VQ3gzF3t0IRXqd53gzu89VaIL1bMTTqU3%2F1M2pQOVSAuIIECkiFk6zw9qSdZzR9OWurEdlBcTQIhAJ8wtS%2B7eRMcrigJz4vb7gPfVLcB1g1ROcmZbtzEGeBufOCoX4XsXyTYipwM%2FziZFt0u8chXw9KuDdbUep5Ywv7XryAY6pgG8Q7eedQ5gFPgV0OC2fHxuCilcfuPzUJyN%2Bix2TQGYaq6uExROLLR4XJVaa9gJNIEc6xN2W1qjwsNr00GXH%2BrxacKfVanmf%2FsjQMnxORWQ1sB1grb32ZkgJswhAxiWze6JPoDkSxc%2B8jTqw8gwuktvwp4YjdUGhZFaKvOn2eEQqWNvwtyHgf2jteNqKbter%2BUC0qLDOoT35XqUVb5%2B5ow0udyG3g5M&X-Amz-Signature=89cfaf6ddcad15b5e94665b955246bbc4d93852e878cca7d634deb3fabc2dad8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

