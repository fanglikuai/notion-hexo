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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBNXEJ5X%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T140049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJHMEUCIBMvnOBBV%2F2%2BvuP1H4QmJgx5hnj5x2y%2B%2FLY8D2tRSbYGAiEA0WJl6pWkWVByZUXuozzMv1RZPDyjI56uK9anOyBPfD0qiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH8Td6dRtR98x7sZESrcA5GpDgHwlsVwWX8hpbvm6jdYI%2Fj%2Bh81zptzXWAhIksKqjabZ5oWbnkAuHFxlo3tc%2FFVErzRIdZ9MNeaGFQkis8Tf%2BI498JVYdmGsWmLG0gAoqILHdtf82VwkO30j0CRAEfmrub%2FAdqMbLLu0uvSFYl8A51wuzQmpha15GL9wChYaSL%2FbLCjR4AonexkwIPvVszmmMhvkwQObC2kBhQGp2uchoyfQ0Ph3dd1QJc52F65DX9NEKWUstxlWX56TdX2ph1nwtbuZdNTBlV8sM3dJBf7I8TiXMStkgGy18U0wvw%2FvAZqXuiaEeOyuqJMq%2F7Bqyw0fAuJKiiuNNFl4ym9WFWybamuxu%2BD57efkMp%2F0blN5bxqG8K3C6FD5UgK5XxoGZxkEkdYtj%2BANDaFQC5415SenWjOgiK8ASLUuIRQj3JSl7Zb3%2FcSomk4JuG9N2mCq8r5dqiN9Dqlfcl%2BSvHZzljq%2BtdfJ0%2BIzlJxrhACwoZIrK4R3jOgRDqsI3RZ3TNBHzEB%2F%2BHgBupCuDP7B1v1770LLUMCnwAXyprMge7lbr8aKaQQtnfFrYyViYqHk6XnB6XKahMS9bTAx10PkYKe20fggjqjJhiP66gdBR00alDPqcDyZ3ex8U9VHB0ZSMJ%2FMusYGOqUBFuRy%2Fn%2FBvsnxxi2xPFPuVe3jlSP4APXQ66v8L6wyjHRQ89Osz3T5VU7iv6iZGcE7xbXijoA3SIFzv5y8and8p376bHzhG9ML84%2B27wncG4n2cugVcLRk62A4EpoadFT63UjEmvO41Xh6IyaczbLdymCln3fq8zlC7%2Fb2eVijEbs131OHd4OQm3%2BuD2Heqqm3RyveTMl%2BVCumIQ08EomxxeYAMFSE&X-Amz-Signature=19d5d338655133f95a8b120207b4b5b0110295ee2d01072ea34cae16f643d987&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

