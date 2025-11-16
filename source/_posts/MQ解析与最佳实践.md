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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667IO3PSPG%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHAY2MvkgpqjlWw3gZy7%2FUqo%2BwixfCmqoRMFwhq2IS6XAiAnI%2BNG1WjJ%2F1htaQeQxa%2Bo5Ub41ZotCuDCNHqnIFZ7vSqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXy4cHO7WOzUMjWbjKtwDD6ZBToq12etaQvj09tY0QobtzTvXfLRxmNnsdy6X7Ur2m2zaL%2FYdSH3ZGgEQxdLVj1k4UjjbpT42Wj%2B4Csx5QJ8GFDb1z18KDY9rr61fvOupyvZtQAjhCcMe5sSCTxrqouIsqEN7LUVuk2c4gfVtkmQ7Morcmbr1zyyJqDNAXwg8voQWziYCUe9EUt9EHDhGqr8RQGNlJDIzj9ad%2F85IKvyxmx2448qtkDkDjc2TYWT3T7KIZ6a2fUnrrWP0g0i%2FgNaCFpvQPtf9yRvA2IC6Q9WccH4Bf%2BG6NUoIp2vhFWIoBMtM48l%2FfvQwTT01WBVEErOyRogyHkT6L1%2BDNMbP4L9Bl%2FnsaW04v%2Be4tOI0LdWiBcnSFIgcJ2UUKq041mOlsUqQSBXRaw11yI4mC63I%2BkqGmQynk%2B49KwCQHAqqaev5JMRikhGxhsiutNgaLqzyQwYkyK%2BenxSGXgc8YiWSSaBzAXMdIDbS2t91%2BZpSPnSz%2BHJtTz4jIAW7AWuasn387F0HxPM2R3JTr17OQI3EhNeq0yhZOay%2BX2SpQ8rftMRJ1cIQK%2F%2B8FrZqFb5qcfOxp9Ga8WSS%2F0GcZml68RJu4ivDlapKjIN2yiFF49F2gB2J37wp4H4Mi93Ioa0wvd%2FnyAY6pgFcp%2BFeZxKLDEb5dQvqxg3htkwwT9BPR4ye5FM%2FyA2XIf0pkWc9TVpIXXe%2Bs%2Fje5oJvMNXnQz0Y6SJE%2FzB%2BaaHBv7WuvJdqxzwnz3pAPwK%2FG1Uas%2FrJZ9MpG7x1UTBD16VLnm%2FyaLr%2B%2BvBwraVeWkYu30wCsGP37w3Yw7w256pfUtP%2Fre1KF5QnMN9IWd9DgBdsRSE9NNww1cYHeb9jcBB8IaFdOrex&X-Amz-Signature=e07377a073cce080909f277d48e2ea99783bec8bd320a2c6db84a20f418be68b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

