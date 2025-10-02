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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S3ZPYGU2%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPDrF6iaJfWEY8YhPQ1%2FnHUNZp9eW5gGjRfrXG8e5qCwIhAM9AYyyP7NdfhD3eXgO5rJD%2FOTzJbK6CL%2B%2BqdJI9z5ixKv8DCC0QABoMNjM3NDIzMTgzODA1IgwCvuWMr77dBdSsgmcq3AO21vs87dfYmpWaakLSeaY67u0j73ihpxqVvrq7QU4Dd3FbH2txVVHI6ZK8uxWrqMAVpBHPUEw6mx7itfCZwbtPmE%2Bqjsl%2BF18trhIGuf%2FbchAysxeu6oN5Csc3NnVJkpuTpjeDgGQCvk9ty51tBPfg0BRubabJJyKqY%2BOoQRLGGfXBll3G6hkuqOQnl%2FA2yNZF7Mc37Q6xCdcvbjr612YP9%2B9FI6qJfGm0CUw5DqrNwuSzoLyVRp7a4Yw5jt5Vk6rdvQPjlnchqIrynbv0N1NHpMUbk5iQFhmacY4OexDolchAGtylgkjc%2FUdDOwEKUnho%2BQ9N9xAxuQjcr9KlrMk4WLrqQ%2Bc2xHJHv%2BKFwQPwY%2BkNNO2hxTbX1jlPcJzw%2BWe9I1renwCBbO%2BJMOTxWLJ1egmeZMp4JiFGw6Tq6IubywCOeGCsw70Jvq1pU0F8fGRrPf%2F%2Fo9Szkly2Gp84gUzYXKwOoc8EV9qJ%2FyD1z89Gr3Z5mQHt5DWuhKNXfvvgmBEXCbtGHZfL99nP0KHs04fYPJDCPTUG1Y0FUZvtfBnXEs8xjT3bZAu1Q6wOP7HZtfGXN5NfGB4VS5PS9%2BkaMPYPYaUD%2Ft8LMcHnjcMgpGK9xdiCCCfA%2Buw6fI%2FZojC1w%2FnGBjqkAdiyzjDnZmwsUopvRvAobR1kK5kKLnMugbgHCBf30M6RGSOefa%2BIE4QHYYysjiRuzlv1qUw0oF7oEGOWGGNMpFTD%2FNP3coLSJ31b%2FXPy01b5A5gzAuIBWwLTN2%2FiZolOxbR6R1a054twaPUPqaEv8fHVjxPrZoXuO3ur7iFkeTs5VPKYilO3Yx%2B3%2BB%2Bd8WTaQqLCi1mrNDflUhcCranWwbpHM96U&X-Amz-Signature=7e0a3f5afaf3917b246ed9db4d4e9ff20a4a7af1bc5fe4182c27821a1d0900e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

