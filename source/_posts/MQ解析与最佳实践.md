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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X67OZNOK%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQDYZYVfKXU8AbZJYoNyycoRp0%2BAokCAPL4zwQQhIi%2FjMQIhANx0ynt7CXx6r%2FdLAJzvTupbWp2weF1k%2FXK%2F7q9YPZOTKogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzxeBTAGy7K7HYxsooq3AO6ilmqs%2Bif6xt%2B2hYlInbNSJ0fQD%2FcMLS9smQNS9EMdvkLftDYc47lZjmWrsg5xxbs7MjAcWszEE4rveAWOiG6y4vC%2BI0pFZEpV4iZlEoYsowyP1GwcPcW%2FO4hnEme70IfE4WFGSipcImZXhxJHr%2BgXZ68fWtD6Z5ZZleJ0Oj4KGjBhLENZG26ZciE6jBSPiR034VGR7N2Fy8M%2FnREg8NeEVSe1iLr9ilgk2T7b%2BK1f%2FsLz6Ku5Zb2SoI%2FZlBhVy%2FoxQJC%2B6D6LQkre%2Fy3BUofuwrjIw9peBvf4gPovAM6r8oU2cbkOdl5N8JOsUADOFK3k3KUgafdOwt5KmGSQawt6HB35mEzQGXuqwqLdo2vxsXlFrfhGoy40%2BbUED6u3VRGx7QwY8R0o2y6TP5i3j%2BbIln1URwFL9uADIol2rb4aeknWtf49lfREfuoE2tjYIHQ691sJQrh%2Ff6i6OSEQ2LizvkspOwM4EPKtU93HLpp3fMxT%2FL2%2Bunakzydig75ZQiApLSARRI5C4Cseudk8cVXIqmLlezusEwqycFPCZuIDenR%2F6M7wIyIstWjknd6ks6cuD0BVZq6u6uzLwSrf%2B0JUFWjLqNWQSu3y%2F14EdmxsfjHA8NnaT4RemnETTChk4XIBjqkAeyCh4AE%2FYTLVeTg%2BHwSalDY%2FabKuIRUquNfLMuDDUQyPzOmIFleXkS7ae4vugxiXGH4dIj6rIvf9pYMrC0bWJNtIFOKpfFvpPh9cSXzu11zPx5hJIfflcw%2FhuEWwPBmESBPdaFsAdblGDa5BtxVv2LOKYj1%2BMwUVwC0bwx67CUTREc9aaS9bXaiPu58JMPQPZlZ2mVyff%2Beh%2BWVqX8gBPsunb8h&X-Amz-Signature=2e5e1261655695aca1629f9ff999e27174fe469514924248d53f1995d794057b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

