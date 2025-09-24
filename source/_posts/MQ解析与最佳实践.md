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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QAA75627%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDAV%2BZBycphITmq4HHF4izO0TeDY1YsRR5fyvQnJalYcAiA0ri%2FEoumm1YDGNP5WzbCGkuSSCSBgiTHrevMHRSW38Sr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMdsA0nXR4l3JvOVssKtwD766QjXh5zLmKmkxB%2FfRTq6xynmNlf9WC5nR7UrN7Ih2Vh4QGBVwmeot%2BAdC9coQMQfkEFdkCYTkxvG8AeaoeJShJTSaEOnZjHMklgACnDts2sG%2BtoOEX0OEnqNa%2BLBu9KFOvCSRiydUEEuKuKxxEUe%2FqmiYLJSk1Zhzw2srmE%2FcjoUb0iy6XtXGBe%2FYoCEft3rrvqtPVwh%2BrBNzeDK%2BPUfRb3ZFF5KCaX05Tyo004mWfPHoYqSMtpXLEF7XSc85900XRemqHV9howqtVrP4jDIIlCuuCi3g%2Bbo8hp9Aznp54ZVhUofd2NsQv8wAdIfuHJp2RRyGrpmE1Ljd7deptbFCBxuqTGi2KsLvC7CtjcwUE3bABfetPZAr1b%2FHKRNmFymFeBySQj%2Bdm7aRKOIvFKdgzbsY9SVOpph6Bv66gyYNockUk4w3QTznuXMdPkq5Ybme5p20xjk4%2B2c8xW1vqhi6%2BkS8R2MxZbog4%2FVgxo0puxCpHFmn9TwLXk2aIBrqvsbTcJmgANkwGumaE5172BBvttlfytn0HBmBT%2F7ptEzSY3ovX4kF8pa%2FGJzrEfrv9a%2BSQbQkDWgyRLxUNO4un3bmAFsiWsTnHrPBu1HgRshGoFqcOfcBIgYlMxv0w2P3QxgY6pgG1NHCgwqoVYhwvh2A2BVwO%2BlGFrX5GjXbrrNcZ3G0GXmLUjWUaRiX6Ce74WrAMNeQ2MsqKbOgdTwI0%2FeB0kDnou1QLyXZfCz5W8iLswa%2Fv3yzIdLM3M1Kt0qKowQQVhUm%2BAlsQ7v%2FL8msZ85M%2F%2Ff9ptGu%2F806z1PkfdA%2BmyyWhAj5XuByWnQ7pcw%2BoXnNeW2KSC%2FgjuulR2PEZfVltg8lauvQ8YW5o&X-Amz-Signature=d7d7a3089cd4f379cf13a6a3c501d24089f821cdfa2dfe7d8ad6fafcb94e9c07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

