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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYQZT5LK%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJIMEYCIQC8Qh1DzrCLf2V%2BZJI1LDCjFwdfTqGsjkZaRXZIJ6CU9wIhAI%2BQY4gTFJgzJ7MyrHOUhbH2voU4ft5eqRs8r6fCq7QPKogECLH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz3lB8KEYhg0L7LqHsq3APORg%2BKN7Dt7GEC87BmQoISVmjMEFBhHz3KgkXiBYt8WRZNQNpJNGewmUsEPDkzB%2FQuQ6nPIpVE8wdLuGvmdg085J7ns4lmTCzO1zkA23HWWR2qxXEva2Kfyqpt5Iz6FUmlA0ScvHXB%2FnGeK%2BqUe4uKc1QKS4sPnFZ4mLeQ%2FCCbMPINHciu6P24mMobfyoY1tzagvc%2FZeMdohRn42b7jkc9VyimDorPj8GYDUoB4BV7pPyttfE0QTU%2Fd6nMY%2FEvAi2hVlHmrRsn0d49JVNfQh2tGk59SeiAxxxotFKLnF8hcNhsA7NV8QynI%2BIUnZfbZn2EBo9%2BmYfwnca5lMiB%2FKUj%2BWImnMR2Xpo8ddcmq56gaKKGLeG0bOfmDVNm3bhIOptMIiHeMfhfZ9x6m3N2jtCF20BvgyrKCy7s%2BhQb2pn5UA8jSQtgj0QCvCcDKXYR7AqrocI68jZOgcS5llhgwQ499pnahqtV9l71KvFF9s84a%2Bijf2SwuTwgV3FJBWsGGKOJAQS2gFPLzy9O9nDSAtVQ7nFRrxlpas30CI%2BsDoxQdknWw2QvcLmiAiVGvX4AwR75EdzO7yUAO%2FsET9dLPcBMu1U%2BwhAgTgc5Ge7eAiF%2FgP2pBV%2B714t6Cg5IjTDu0JbHBjqkASuNMr82j%2F9%2BjjabbLw7NE8e8KrckBRmrpL8tpTizhtrZFbomI2QZQdaDCIxodai6bHvJhINVCP05htVY1pvFSqT%2FNgndd102UlMpqxCGlbpeq1MHiNpd8WR%2FU4jv2kmJ07bCuYlt9rrbiuS%2Fvt7kbgvSNWbbGHXoeG4mEjxpPsIIU5AH9%2Fs0ft3T911r67DjHrtp82fNeDDJpuVRc6s5nnfbRKf&X-Amz-Signature=e6b880ee9a7161055a4b4ab09f373c446789ca2c00286ecfee25eff6196c57eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

