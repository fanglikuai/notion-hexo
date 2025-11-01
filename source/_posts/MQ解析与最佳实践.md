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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XYPJIGRF%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQCpXhTE0GVr8sL%2BIwodqPHlIGzHF10zTEmjRQxWA6R7ZQIgJAaUi90OtmbqTVii%2BRNoIECMHO%2FlYCrCXivqYwZNk5gq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDAffxJC5O0TlHfV3QCrcAwP2lJfNyBJ11G44Prpe0x6shbuy%2F2gn9wAcoEQIj8L55nMu26WcSqi9MeaxW328TdwgcQfNbEZLqUC1sDXrdDk0nHBYO7vs8kEQlNw2q8%2BWkYFriQ2OBJ5kInoh5Pmz5EOBGRTLtmXSEWNbdOcbCzz3occXODlgaekjZvSoDxRDFOHWP2A0gCw%2BlakDL62Vq5VgmgD9M5Z8Dqci7UIGBCGYYy1oknWSh8Av%2F8QXH2TLXjAqCXPQhw8548EAYt4Cxs1%2BqlfuKw5kyYHJxV71VM5GXYB1VEs3GatCPjQG4P63O%2BuTAgQhQNdq3gIiy8msPN8psSSOUzIZlnh%2Fh6WiBKTwNIuEMN35GOq%2FT4YXqSm4znSznYFnjrOJwpZR8bXfHavbPrOrOQVhziUqR0gEYiMvFXN5ijQeZnehIe4waHzfR1iXtfYGJt66YnOqarfifKDsMp9YAte9Dy2cnp8bnO85s6EdGmygtEvHep%2BkM7nnIAfFoVMogOD844xwZOJmAIbeS5nTqMgNNLB21t3RLhghpxoGEo3OQiwYXvxECYpOFe142WpNLq2Ql3rHT6McaOyMR6FFxb1B8m4WeGJ0Ppr3M6zFS7ZMmDxSthafzEy7ySOQbSoim6tWMZRLMNL4mMgGOqUBZZCn9YUhx%2B6k56ox750bRMIdQwP%2BpkENjEhUtW%2FCVB6ZVghXPzR%2FZLDNhPxeuaTDT%2FH3sORImhozHWQSRFcY7%2FCmMqTZ2KjLdsdSVmAcfs5eqTrDMLxIScXFR5cAZwxmeArwar9or2IPivt8uj8Xbo%2Fjmi0IqqoxzVivfQxXixIuXgY1Ap%2BhVlpJaeaeqehkA7Rfgy%2FSveZMRls%2FuJi48AuIpEBE&X-Amz-Signature=9b1ab3dadb77276e36d8e6c7c09d812c9f84d6dc6e86129b4b6aabc3e91feae4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

