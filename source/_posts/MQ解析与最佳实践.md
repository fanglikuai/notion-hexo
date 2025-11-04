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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5FBMHNQ%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCqC%2B908QnUde%2B%2B6UUEd4zWUD0Yj4Px%2Fx3Hoi3Yt2gpIAIhAOiHnAwiaueknly52ZUSXTdMLzrj1eL9sN2ExUIfp9pMKv8DCHoQABoMNjM3NDIzMTgzODA1IgxMZhHmLUBCogB4%2BFkq3APNirZiWjyVInFM9hYS%2Fn3J%2Fb%2Blwt0SeazCugndICIZwWVw0CmZVQp3GdVsUpQ2Mpsb2Q7nJjcMPi%2BxBGurTKjfoZvkxImsjwCw9FsnVDYaq3FzkanLqw04INaQuj8KqRspm5aB3NiN2Q7BXH2q2%2Bw3Oxfq0SZ2Eh%2BQxlVj8MLDpJ8UW1K8tGzxMDgSDZWfVNs%2F2%2BuP%2F5sZGv3oVoonisWEXNLfLqvmOK5rLbQdHXygNLuIoeAahTNyXAN29MeRH106Kshdoxnd14WFBchM9y9Szn9CobyiIuLUdaYc%2FRRTHEed8l57F8cbZayvyEDfgTuzLKutXQwhT7yzVbDLrwHWVvxaqQbdoIkqX1VCWXITZLbkpEq9jYKTOChuL0B2zIZznfKjKn6ggMytsIy4ZvNig%2B5LVx8XOmDyHwb7Nav6N%2BjD4dwwGBVgDhF47To9UkAZlj0Xu8vP%2FqShv6FziP5wDhqV9ZzmtvjF3Vpso%2Bsn7B%2Fe0PQyPCEvCNZbEudnMREMmC6WCOfq9WiPc2tBtdx990rDtuTMCc3pwGpnyGogYhawB9XpHGkFcfXeMbQ0kvLbeUCsvjSigGf72bKRTi5STHkyCTun2InMw5MNPGMix%2Bae9ELhOTSFYR1DdzC%2B36jIBjqkAcTAqjiCxGaivj%2FQNvWFcLEHOgbj%2BxYf6NnciFRcKishn8jmY33wMsFz1UncEkLCYYRZp3fakG0Uel2pKLSrV4exdUVscUaVRLLpiQSrPah4NdPJG94RUFOxPO5EplRIgsmT8uHYN2JMsHVCJ0tkKs1BE4zm1MF2%2Bl1f0wgTFWljNy0%2FGZW5vT4pvCqRHlVOzDXryRU8qidy4zDWiLM%2FA4cK%2B3hI&X-Amz-Signature=85901dd8c29c7055db7b73d83b58cf951451a839f578e70348d74cfa2ecd793e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

