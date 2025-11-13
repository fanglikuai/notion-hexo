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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQFFZ4SO%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCIF4ULlqV7IQDL2Lm49nSBtX%2Bqr%2BuLMM%2BTtifucYu%2FmWOAiEAvQ0AK4Al6zZdLD13vKiRTtVfJRAy3Cr37cglOtUxZp0q%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDIR7fp8K6787A5NXCSrcA09GpCNSvSCyjJh3WLqIYy8QqffZelvcTR5q62SMHpQFj0lvuXj4CbrvwBNSXwC7Stvzo3PwkdzANsL34XjaCLiMHQL6YncaJ0ca1w4asHYA5kBugwe3BI7UX67XB3IaoSXd8zZWRnCs4nLuKP%2FKXb5BG23PL4cv4kJmoyCrhIXUsGBnWNqN7MFeM1ivMvyUY79PQu7eR6yZGlLjC%2F%2B2cerO1SQ9xDyJY2q0HyqDjnR2Cx8JpROs6LxQL4P%2FApd5qj3htx5DVk35ZQc9gz9rsN0YKbEkphvoiqk72pEzKSWJvD%2BqN9wIBkjsSY1n0n4pnhZQKC6kngtz%2BeW4C%2BEWt6%2FXOQCqAlAXgT5gyd8tl2VdZqEMPURYngj9rcsnr6sxpXrHopx9kKfe9uin7ZgMYr6b6oLwDkZTtPnB%2Bg%2FJ%2BhyZvFy1LvDFRXaAzcROnWhTcYZ6p4MXrY45wJdvPu0fuKqmDWy556Em5cVZesCmv4dnhkMdP6baCq6n%2BGl322mKDrizDslMp4Wu642ahtReaOoXZrlAufPTicmpUTQhmEPz9SCm99gGLQo9wTaZ%2BfstoQoHJqAFsQEP75SOPU62ZiLNhKZtSNegGJPW%2FPBB7nRdCBNSe2PzfvhPijG1MMS71MgGOqUBOj4vCsjQ%2F8UbTVLHqFsVDWCa2ykjV5ZGaX%2Fz4NdtdsOEOVegxqdygk3LEfx3DdGpylk9ZF97Xyn%2FGSzzNDds0g5iaR4IBmBJ9YW1bgwTmS6ylIugQJ%2ByknbzSiXFuuI5yKHdK8pfbQTh2usZZUUUzL15sO0%2FsR%2BevslUcYoGvjky3UoWuWbLoXncsC5%2B5GcXGcWm6ugy2QNbmd7HBsor6LM12ebl&X-Amz-Signature=827684668bbdfe8db7fe39b2369f5af96432769d30069f03e7d437e25cd55f8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

