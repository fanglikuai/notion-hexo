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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPUOV4D5%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC5xL6r6bYPCvAU5lK5a%2BnW79smD4JG4KMzmumhdkgV6AiB6eCRT1JaWj6MSiDj1vo9JKUTeP5%2BGECsSh6o7gWNtxSr%2FAwhpEAAaDDYzNzQyMzE4MzgwNSIMEqQtv8vsScVKYgHBKtwDGejDMgKuRZJU6caOtDtEcheY61csW8is86Ryh8TbkINDHbaxw%2FuP1UEvXbAl7XCIIuKoLpVRYk%2BQW%2BSf4fDBPrCaU8h2Q1ts4kqGV2cVsZPQ9kShkx%2B0f%2B7qiXlPodO2vbjXY18Nl8qlLtt%2Bw77wXVJYNi3dWTxTLJ2NVMzb5k9MgEda%2BXlm9VxhX0RFsFS86%2F%2Frr7jZGWs6vnPmL2yESTZBwxJFpRB6AxVhg0UNy4B7aiLoQ%2BO%2Bkeb%2FQDxeGHwz9%2FAYISPWk4loakzBADfEScGmaOP0xgEgAkREJnlbQ4VoNfdJUkiMydmTGflOJsnDi7hY9qE3VUb1YbmWJO55j5%2FWvy7eS9tnHod1l2zgDeXVRyWMoVOs1qNtmZ6h51Vibe4p4E5EduR7huCGZQ0xOO%2BI0bY2fMhu8zP25Y5DH29TCgeqH2LOBZBbqXI2qgBUddgv%2BYx7EvNvOKkJuF1Rs3SAM9wTEbEjirXn8KbWmmzUSYX%2BZ0wuTs3fIpS9N8sRmyw1qDM%2FHe0vMR3NyQrGYs2ClQPhlXdZk14swnzQMAnLW69lh9Btm%2BndphEmV0zCl5spnNbH%2BR377v9hJsj3sHb0Z4urPPgUhvJ%2B2TrrKZEGLtjvOBOqrLCkSnswi8S7xwY6pgHvCd9LCSk7Da6dpBZQxwzbF6QbIn1WQwsK21bFSqMTHxtDQwk9kSqN9h%2FCw6JPa%2F7vugcx%2FbGVYijqttLpz9rKTKxhAvje64HxWkwhvKU8qmrMoUhzzb3NS9%2B6rBz2wlC2XIKzWeTp01Cr0wrEGylZND1pfRo4WCjsuESo91VPR1d4Jy23g1sM0EvX6ccHGGXQShe5HgfI04mZX6gRHdSZhmoKpKxL&X-Amz-Signature=3cd890fe9116211c0f360091dec90303c058e14be32d449cc84a4300165336eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

