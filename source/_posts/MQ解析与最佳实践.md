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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VHMN7VZ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQDCggtv5zDkDV0bA8Z5RkLMEm%2FQKexncE6swtvq8bmaJwIhAJXprM9MtF0BTUKTIadyolOYS4F4i%2Fwq8zHnlI7NTLM1Kv8DCDUQABoMNjM3NDIzMTgzODA1Igxj3w3aM5IJhzJmGg4q3AOP3L%2FBAo5zeGUMwTQIxowtoi8oj%2B98zDS3dtc5h2StVLwLgHpgXizmAkHpvZYyfGICSmEoh%2BAOBP2dhmfCYsNfQ2PKq4JAvIGtFppGydJBdafcVOBKu%2F2xKgFFGi22asnbngA%2B6Q4Svf3nK6CyMzt3JAEnVpxlwVGO9LREDIdvj5g56P7N8dTDJZIFZIT9DvIgtlhf9%2Fr10f57VTWktCCu65Ig9TXbsxwMVbuNR77nCbwI3aD%2FbEsQCEQtGZiSjznKnUINC65UEU8sKz%2FHvMBWi8xUlnItZGbEKpmw3xotdEktOe2GExnPeWLTNyNuQQ%2FewBH68cvsbwV0Om8S3YEhsgvebPfVop8G7oNkjtJAiLp1%2BygiDqrSDkWvRkRtColuWpbWtl8aaiolu1lWYrpoatB8Qsj2On%2Bben8Scxe7imTEJB%2BDykAoRn%2BKjszUv54TS8Bba07oCzjRfikObhzLngQMIuf8Dgo9qkwt9UvpQy%2FZTehfxRhSgoQCo95qACktnq4U3DTS3N2eOjYtKEqf0PrLZRExbeAljT%2B%2BTUYCqHxM6W8ADl1rRgJPuTF9SmSMmKw475UwR8Coos%2F6cxe3YuLkY0cHibSpUnAQ1%2BbRT1bSZreZMcmn8j%2F2RDDJw5nIBjqkAaqMRNWSB8JSpmQ79nT5IgFIz0Cw1ZtVMwJxGQzc34pEPvc7%2BAFM35gpWyEU1OCprcMHsQUmjlZy9Il71wmrKMy382vFfDuRhgmqufeLNvLLojScCKANSA%2FpOrrXGShGBE8qHjvWz1MAwtGJfa%2BJ9tFNg1RLJwbDACVTnjqIAyz%2FfjFDC3vEx3lnE7%2FAhZJu%2FRHmRd1YCvbn%2BSXWSg1Z4A3tarua&X-Amz-Signature=8ad0ad14285a903fa8e707b8868ae51fb8515097251e9f9d152386f84124acc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

