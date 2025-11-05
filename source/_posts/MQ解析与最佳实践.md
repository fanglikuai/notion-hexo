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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KICVL7F%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiQKmZYWDL4%2Bx055TPq%2F9ogxs9UJ9ZKbxnkesuHqAoNAIhAKBGHaLJ5e2npbIZm8votpiFa3w1a8piy3tRbfrYUay6KogECIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy%2FfOHY5PSv22jNwO0q3APjFVDQevvNbs8TGKcTYk2JsOStIwJpr5vKFFTFGTo2rDv0wb%2BNuFKJCrtbGa4BL1flrz1al0DGj3fi3mcIWJxAp1YJG0KVP7GbSU7fYGfh343vNt8qh6kmOcUXRXsDR%2FaO4qvVdjzigAQirBylLM2zNJ7pcs5qPaTi7PPNys5rZ5wLVpyEqur9i%2FuBC7vFDRKM%2FJOKwd5Aiwf8YjxjAg5Kr24jbUPMWyt7Z5nn%2FFgjnSGnpS13HKxYJlwuD%2BWIEN%2BHaNsRNtSscdA4BRa8Ygk2Bg8Tlpq42hmRrP6U3IHq6qA39%2F3uCiifDpA6lU%2Fn%2FYWTtjJubl7fAywSBARUourUIrADzYd4mCCR8aWZ7Jdd%2FptkIhzpcOxGsyMx3fau%2BqNGUo%2FbT8FnIMPHeKrbA52bbWbA5D4U%2Fj8%2BbK6PUH9eKTLUHOqjhx92sffRstPSyZmBrQ8awmrc5qV43RvEMSAKYX34neQJ9e%2BDBS7im1YTs7XpW6KvJ%2FHJPAjE7HFiRRW03dKefnAdPXyfJqUKnv%2BUTBiSMt6%2FplrlPz%2F5Yniqm5PXHPtjgv3QbxUfyGJ92u2nFZ6Xwa638ApkA2avTq6smAMwIRAAP2tkmxzkAz5%2BcNZvO2s0z4shx9oJ6jCR%2BqrIBjqkAUzmo%2FFrT77Ax%2BM9pOSyJKInYbgAvnwbg8Vyg7lfFlY8%2BEGsrRNylBql6p5fJjvZFWsaJpI2%2FxJaMhkzQxq7Iv9203tyB9uB9cAunHD6qU0JjeijGbMdhzLzdNDzybOMxu8u8SOD%2B88zl8oeT6RO245znvgBapNMia74oOi8MpQgg8kbeIlz1uSGjnZ4MmjpLPKb4xDfn5qzSqwpqbYT3k8Wb3LK&X-Amz-Signature=0b4cb670b329034bccdc58e135b6452aee659d5818a1f78840f69b6c5fabb41a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

