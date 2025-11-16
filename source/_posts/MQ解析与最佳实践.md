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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7UW275%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCXcnotzGQXwFAFoYtvOOXVJhZ8cUJF%2F2EjSERtlibGYQIhAM7CRf2QIEeXoYRVaBXloRqi2Ft0%2FK4T%2BWAwE7xfnelMKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyp8pJ6mMjYzUV4Dcwq3AMXz0KCknweW4tVKu2ZJOPRvlp4yMHsso4YZcXBC4h%2FJpdR5dlcAxg3MhilEKgs2g6AHGS9HS42lU2jrSDit3UVZKHONmCvA%2B1wF8ik1dv2DK2qcb8jyhXVCHtBYhHUyEKNnnTl8iWENHrWW2nj5vHdyff26Mt472Wwq%2Fi%2F7plL%2FYxIoL6c4zEmgT3%2F%2FL9pAa1eOY%2FqGW%2BfZQv%2BG8x4PrWpJw%2FWRat0QCTtTRIMaMxh3y46q%2FqL17rU%2BrQp4JhK77MUj3tO7fC2F755%2F2NTJB2Dh7yQ8hWstVIPj1Rq57phjvK2xIF6OLTP4KvXVJuFzmqYFSLEoP%2FaBZl7AssUCyxblDAa3CCTxJjFxKWdmuXcL5e07g1MzDn7JGEh9DPH45CACFkl9MOjDk66Zh9M8j%2FlNSdgtTQZF0%2BbgP387iX4vsZOYPE6DbMVXGGF8b042Oa6xKOI6PmKr7VTISHYjJbjvnV%2BKKJp0TKmmJEHkzufs4yM4JaJrpYL0afuo%2BKrnvjGRSPWSakdVjC%2BO4zCuufZupO%2Bmc95yq%2BCFXW978m43wW6VYpF337F2L%2BDIxuaPt7sd1bQ%2FGSw2jq1L4bhZOCgN4QELYZV7YakYNdroETP6xGEepzfP7S6xtPE%2BzDwzuTIBjqkASvdmKNtE9dKgKEKmJwOhl30tT2pKA87LKfIqnlWXhA2sfZoPet9POsTS2aC6QJolmrMKX4wSkkyaBNYJtFi1Yfi8m%2ByD6MqtwyHB7qzvkCh%2FeuJ7Y95p0dpArss1cm%2BeqhpESPnCxcGyG%2BQUbKlmaofCopXFxJtiJ0%2F%2FPq%2BjhoAA2kozc93dpp%2FB7k79jeeQWIpd5%2FKzSLuNiKsohGmRSuu2wCY&X-Amz-Signature=f44f9af8dce0c21ee15e94d69cf06a13b134df4f9a40b4d2036033f0059c3f6e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

