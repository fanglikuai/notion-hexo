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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTYF7MVM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFw7EK52jmctIC0DXq3VOigSMcce3Qd3XlLguRZ51e3NAiBku8ziz7rBFDpwSH7Sr%2B8QChwswKpszbUqlN793sVKvCr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMK8fN4%2BAozVzCMNvtKtwDFVg0fFMV%2FMNYRdnvdU5M6A6SBRQZ9cPoJIDaF%2BxUdiQtyOMqf70Z2WA1p77CLUhCau5vnO9PLdUsK5%2BIfYbsS2YTUTFV%2FqzBqN8SQeRax1uYr9AsF5AgD%2BIf9RcN%2F6saN7vfZPPtoTayY5nUzbaF7Qn8mb2skzGAs8QEJh0%2FNtLXaY4teSiRY3qPWHujrIZCPFvnmdlD2aVO3sJnD6GGCnsPSrqp0GZXJ2k%2FtIePC%2F1mnj01zUJPNVRINelrUq3UikiPMS2IY7Lh3AjM3lo3e5pDxozgwmVcQJ43wsPkesDU6pQ7fOx2XP32L0QJpwWFPT%2FPsblvOG6%2FWVRPG7QuQYEfOoxkjtg46lpG0ehBqjRQswbMRmD8e5YudCOHhFI2Syp%2FeVYTYE%2FpDEg8G9g2lZPnYbKR3b62Y%2BoHPjEk7HRZPyH%2F4GOd0NCVpOv7vJmqibdAZ9h8ENitfs85yVtrGMqwR4lMnpZqYLXlZ5Mvp20nQw49KB8Z2vDl%2B4oOFvRaADZ51XZ3orGMnSM7WwpVsTmurhaMPhWHc1uE702cgxlsUi9hwivMVb39NmgrLne86aznlz%2F238EUNjkVQH8NcxHxBfXK4xJysc8aoDB%2Ff7x5ynYO77gphnnqKxkw2vzKxgY6pgGtQcbhCXmATf2Ccm1kBn3ufq4WxeaQNmLX71Z11hk1hLE1VjB67U6GgI68WKzQ1UrJRFJI%2BIpeVxxwcrFSAGRHra7JU8yEEE%2BlfkiQ3GrRNVWRmXKwfKYLnE%2FTVsvWrjsEELVVrySKZ04QbD6VLpZC6uWgnoQk35CnXPaTbxItwsWsmlnt42oRrPnI6wa1uyJu3tD6DzPsH%2Bhzp0bt%2BSHS33AQ7DX6&X-Amz-Signature=419bd821d9a55440c2308b13ec40db4ffc5504fca69a4be356a2bababe6dd03a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

