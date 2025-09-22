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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMTJAVMB%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T050222Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDjTEbos6RK6c5UqPFeUM3nthS6es0mrVpTAQ3hTHYhmwIhAJ8%2B04bBanuuiCc1q%2BjZ1956oaDD0NoCzt4J0az9UgOPKv8DCCUQABoMNjM3NDIzMTgzODA1IgxznXBwV%2BSuRe37dR8q3AMRqA9O4vnK54b1uZc5kINoO9SodAjKCCOYGbtQPwwAOkhW8p2GVjLqeEr7QBCERVHy8EqvYe8iVDstscCvBm5pw2GRLSJumXo8K0JEc0LEtCU%2FZDdcMWGwtQUbqSiJg5IGyegF3YfN%2B9aHeEX%2Blx%2BDp%2FRkiWhloIuIoRSi9mTnOSWG3aDP5AgoUBCHPwZcgV899c4t0m%2FU1PoP1Fp99Kbhmdt0ozp28gzieIS3nA%2FfHNoe5ni98vneStK2Ku93hOvQXsRHtVMjh2VmIiyfKUQWtiqllC8npkvvyiMMZ6qLO8ynNAknLD0pYWkAJ3Wsd43WicqrAfzNVJkxjwQ%2B%2BoYjvuIacm3CDMa5HyacNh4AZgdQuuwMNuyDtE7Yqo7X30VILx5q73eG9b%2F1i1%2Fp3j3R7XlPRAFo8nbMmk73G3%2FxQuC69wQBycIe%2Fgf9teDx4NROfQoTT%2FsEMjDbIBVYxbb7nCZRSYaMXfhxgPWzeMgLVYZeAdXynkTcQQUWJaoZCeCVNEHtdwLC7lKZFEpm%2BjEmONXxpK5bTRUf%2BFogvb6xZmqvbCplVmhpARaAOe1LxmHeLogAc8CNd5aKb%2B%2BlKp%2BiX1j3i3AMaRSVVEVM8DAAp4bEYiGm7QM59%2BN8zjCrk8PGBjqkAd1ICgrQut%2FCPMoBkH8ufNdt5wdDPOfwp6aTUEireLC3I6bFzfpFjOHCT4fPycsggInQgc3DpAMOigeCKSMtaWaxBtPTr%2FP6QXNzfxJuY2a58bHRdpv5Ox%2Biesvn773JmiW%2BNyafCoanuJAOCHxNsKKHjLKT6LXVH72v5IkNAoLH5%2BD6JQ5M8jZNAEU9qAT50kc53hM1642ggJM01G70D46rjePR&X-Amz-Signature=b11b1ece35644f5bf63c75f71ce5a500bdc695568280d46c62894fa7fe023dd3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

