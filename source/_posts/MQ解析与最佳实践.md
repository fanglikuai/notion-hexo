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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664IEY2PKH%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFqP2L42%2BT9COB%2BT0b8pY%2BvEJsEkYTccOdO4rVWTqIg0AiAaTzyolR6r%2BBHQ3qZTV7N%2BTMFZPCg1djhs%2BYqBmw%2BPhCr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMyG6pDGX4a2hVh7KkKtwDGGzRDa98dJS7C%2B3AwOlfPF0iSsCjgbDi9m1KZHd2GSiBCGpkn0LPPwEs9QtO6nDz75Iv0%2B%2FzHjSkml%2FDphBJObTT8vBSNkZ1CY5qppvp74EBQJpLOFeGfPQ1j9KYVoQLbHYRW2uBnJwmYWDczn4QC1AQKsKUx9IqHoyMKS7Pl65JtjhGCUPXjzgQnplFrhTVbwG6sTnqwFE5plYZew%2FmJ4LTN7M7%2F4oYs8N2SaiiuX%2FeyijTplye7YozklZlBePew6wyaXC4ep%2FgJBvzMKp9sBcm2gHimXplcj%2FFWyINXiOsowO6Dg4lIvPIabaDdkaEUwmMnTt1J73D5lal%2FtcXn0Mh7Ps5SwydAmi0dz3txPGo4HypPDJceBucqUdcRPmvrwEAacEoRIktwzya6hbuIClsjyF9A3IHxCmBXbDnRD6tSYcSOXGo4g0qgvx0irg8BIYCQA3nNRuChBCtRcINMMAqGhB4oCOuKtizYY9Ig5naImLuYAMiMFBRpi4t2zRMAz1EmRzLPgAkqJ%2BzAzThM0oYNq5eAwx6WFJDS7i0ahpWIdzoWsQUNFdjel4E3OU7glu6KsdGB59z%2BqyYN0Rks%2Bpf3JAvYo8jEyalDreDdAbB8f5xjv9kPUQTlSswpNPeyAY6pgGggkYAxJx56QsD1SawE2JrRjENHDVbd9gBbJb2wS5N8%2F11PdlBaiRcNPU1OrP8E%2FMatges6AsMLYhDOZjqu7ccCUgtFkZuR3T6D%2FNnrRfEDkLLWae4p%2BhlOdzuqTxjrqn47KQehkSOjpD8f8OXcshJmbTFTjCR363opmYbFC3p7%2FdIBHQQrNq8r6z1P9dwyJ23o93bpFPsL7aPXiiqSNiBb3wh3CBs&X-Amz-Signature=2d501b53f166069a941b2d5b01ed0263f654a9544127564a61aadbfa58e95425&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

