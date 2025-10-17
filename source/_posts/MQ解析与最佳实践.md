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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633O5X765%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDmu3lP7QrLWYopeC8cI2JHUPUQ%2Fx%2Ffk8mOfjHL7KUjbQIgZ00JiPgnSxVqoOZ0MZJ7h6vgBWW35lP23kLwsDh2ZQ4qiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAiIyf3o00xve2GrlSrcA3vONDuXvKWNmykNqK%2BeEjw5FffHyqM3357aHSRN2sGTRGCEAnRV%2BrZEu6JJcBTe%2Bf34axLeQ6ZkC84SmGX8YLchakgtxeU29Ii02ic2F6n0TGogiY0k2HFDFnw5Z02X%2F2x6ioXCfMAaPBSlR28SJCQPb9aHb5FG%2ByczByLd05xk7eMzTKghR%2FqFVuTqyGVmReKxZ2peyUcb5A40bL70wXkNL2%2BFPjxekk%2Bk7AFCJcKSZwlwL24wLzKY%2FQyc9nvxGr4KSsfkWmKJatNr%2Bct8m8XtpvpVudYy4LWeL9Pt9Qu8tiJgokEX3BPF3ylGk%2FQQTupomsdRvtjsozJTWSchzwlS9b2%2BWAGhXwUZK%2FQ6gzu9%2F4vvYNaYF6a8Iiu1yCXxIpmJrCuVlQDIhBpyTA%2FdB6ilaT1o5dAt0OfHczIH6QQvnDffDtyWSSK9BWxGYYY%2BJO1Z9vXw0wUq036g5%2FJS2K6HAsaMmEtRKhh7GFQk97YB7z4HgEtXML3vTffglI6vBt5fhvxuZAxSE6cjuvK%2Fu5m3nlZhDmDOQMrHXGHYHmR%2BywdPV%2BArAJIHhR4%2BXWWVuQB8wIEVXVf0z5C%2B2LM5kHRK56mJXgHxbP5jpDGyPynzUE3lihmrAiKj2k4pMPOEyccGOqUBu0toGS2rkH8BkpDOYvttJIpx%2FrtPGN%2FYoPy15WvAtnMF19AB%2BjecspUbtjQqgkn6e54xZ3%2BkRPY4KtXy5PXJfjEjgThwbKH8zcz8m7dXS4oC%2FFbsbCu2fJCgn6zzwsLVmt37nRT125y9dUFBm4leAuHp1EB%2FJftf3sT41tA8mffBqjETN7c%2FIYfmZfB7%2FYZig0iiV%2FvcbSD%2BEdcUeQCpW2c907z1&X-Amz-Signature=8514fdac16286450dfc63ec48ba01e61eb44af254e253db7e7510601f5fbb664&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

