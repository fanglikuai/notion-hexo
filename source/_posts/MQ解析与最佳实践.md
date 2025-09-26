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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CVJ7DYS%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQDfg8VLiDvFh5IzCtI61Lv81oiWIEILD%2Bj8Fm%2BBZevhMgIhAKzckQ2KeIYkhuxbCI3ztUKgR90vCzF2odRfS%2BrbfFnNKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxBDcOtTvJZp0Sri%2FEq3AMJgiU6dOt0c%2FT6rHb7aB4yqK44aGGzLiM0oHjrRmKkxs3WPDHO4B5chqXrM9wdUtySxaEbxVRWTaNiQNsenlhoRT%2FUmVYT8jXjDTU8YuFiXlRNXFJbFU821ZCy4%2BCRFIxJSgistCq%2BP0EZd0NTVp2OgwnZQMbULK8i8FqOmTCrwtlH8%2Bp4woa0W6yU2TnmQP8FYw%2FgGEYgajrbcgwrI%2FDy%2Blq2JncSO1FOanXrAzQKJ1ZZe5njU3Dr063yEW%2BMYaRTRwix3o0kgyZRDarSqyWdrdU6seA%2BnBjuP%2BeKfYo%2BFwAqxdTwFIvSAWpEW%2BqIR2Mvr1tpMC%2F8aHlxhsBeGX2mut1lET4CvZvqQOlLDi67iYlajleLWqUso9wN7rnixZuAIWdK24MuP2YN2JogdwHmnDMsV5jEePRiwkapSuSf%2BcizHs80UZjGN6DHdpvywD8m90%2B0u5a4Qp8npVtcirJ1IHhdgvo1kom%2FviKDPuAQg7HRH%2BxqkFJQh55MW7txvhB3eUKv0Br7gt069D0unm3sXY5v0ZED6XBOIr%2BFLdXQOlGVZsvKsrVW6EuRRLcR8%2FhhnPg4lxN4Au8xnpKid%2FYHHFBE2TP59BfQwOQLImhxKpZ9ZurtuEE%2BDX1H%2BDCy2tvGBjqkAbhXefOUCmUzQXNFKeB9I4AsZpFyh4t9EAhl0h9U5jT69%2F6IW437IIho3bSK%2BZycpFwIPhyozp5RIznI%2BbI7nLylzggogamhBpVT4f%2BNfhT8XgzGWFuyfBbAVz0R1pFaOy35FXC4hCSvuECb58DvidOdpDHZz4yYozwyHE40xZKXi5eGMFxsXABmBPq%2BaMXtKeQcWQhHIoP5toBbQzVyRpBcKSDk&X-Amz-Signature=ac56699d0c7ed1b61d584ce1bf8c1db45261ec7c9ee2e6a578ccf5ce6c04905d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

