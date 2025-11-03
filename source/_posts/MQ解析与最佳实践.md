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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675OY76NH%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFALxfYn59Q4%2FiZxlKVhrUpT89WjdR98Rw5xnsCFD3QAIhAIDyf13O%2Fd60CBG%2Fnkor95dNnkrzlyWC6Xc%2FlBd7QSKDKv8DCFcQABoMNjM3NDIzMTgzODA1Igw5YSsT71wgwo%2BfP3Qq3AMG40q5e5sH5h8neQ9CHP00Bcu8zS8b5VZ6ieIogOF17FcynxWiG6GPUIAlniEbRbk1yI54T7oP%2BiY8GFoJ%2FSU7XbFOA1q9kwO5XuHz2bu5%2BxwfN2kanxzI91CCfP55g1UqclpSN0NWNX10jwgyfkOxGxZfRPDuVrVqK0dIb1bKi%2BrIm49gsw11imS8o0OdptdtfatKeduyboOUDn%2FDi66xmJ6H2XLuVhwsQkwSFaBkvh578gO3WeUbC2d78rASVow8xd8hwVMire9YpZMt%2FBzH2C4UZ6WOMfwM%2BF0Aq2IlgV3Ou7irnJBLE2C5z3e%2FyTXxAr6VrBw27QkSLsbo0aUr9GNbIt%2BnvxrNASetTUNMeJWwenHyPRLQg29IYLfAeds%2BGr%2Bt19eiGIFZDeVXC7ItwWF7DtssME0PYOtn6bOinPApkiW7k95oFU19PiSmzMkMfU%2FOzVkwPNB8%2BCYx0BrfShQRl%2BdP6GAlXN1%2FnevU0pJ6VqYbci66yi9KM9Tnw4yMGuJdyC5jwiLBfaECj7RP17Ym4lL%2FYEn1ihM78xUX79Yknj5YLLt%2BFAezixdJY3TaGyssKh62WArLrf0GG%2BpOEHNJlC8ybohQJ5ES4OQyoFrasBbNwhA%2B7e%2BQEjCYjKHIBjqkAQrulMchr%2FTsB%2BQdGez39oen5lRZYUeQRNcruJaFG6E71IP5YsxeKsz4si8pcjT66Q5Mu3tyYxengiRiYSsmvCJphVVJ3S0W02bfmzOXDnpGR4OgdzpE3qcMOGzMqQavGx0pfnbrSHlPJq5BWLe30HCXaohf4uPqnRcaT%2FyB%2F2w1M5o6FV8sXFHuu3mFIFPLWWDvcwDh9Hw6W6qa59NvOD17jDZp&X-Amz-Signature=a8c92da36bb0fe98c0cd2944bec73ac3a2edc3e0136b6ad4b845c1d3dd93261f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

