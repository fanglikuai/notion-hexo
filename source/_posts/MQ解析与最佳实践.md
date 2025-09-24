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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EY5VQ5E%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHbenExfNHSE29yjRNVWme2%2FEcZ7zBspWVMezTq1782fAiEA%2Fzw5GTy6Lo4kFOHVSELvajPYiMIk7tC5PII19Hsh%2BIUq%2FwMIVBAAGgw2Mzc0MjMxODM4MDUiDB6UIY32gs41bULLZyrcA3oB8%2FMpK09W65LFB8K239lEGyUkV7kkXGz37xQl06PRtZ01NXOmgWXmmg1F3iXLwzdG1SZ82p2IXOIYS3aDqjCvdFhiORtaJZo%2B043whc7ovd8FxVu47PmTLb8NJUiX0k8%2Bfwqlpx9CVgdupsvLVQTmDIcwRChy%2Fe8QLacbxhRAxRdaS5TfMBiuwe1spCKSQOH1jaMpua2mvl8nknQaw9kKrffsgyl1xs%2Bj6LzABdo7QIWIGTKlzt9b%2FQKalYV0Nv1F8rg3biLH13sn9mEP4IotpEGXeXgg%2BSkhn7%2F8gO1q%2FzfDeNnKgXosCtmRylG46wtv%2Fjr9jqC346zkLfJKz25f24TRy6UUb44YLLtjnJHMQC843DSuYM%2F8kznPLbYZSM3leh%2B29%2FHLKFEtTBA7%2F%2FrHyeCt0rlbDU6UYmRSIjzXhWkYq3LDQGNqhlI%2B5gzWYXevzyxculxpv642MK8xDtonK3RZhg0JsMiFNZUdMiYWaOwY5kkEqyYm6cwmAzPZqKOO9VUne7%2BhAJUBH69o55mHTa4mubWFoqK0wkhrr%2BPKqNMrUwXI5iU4kob19IFJy0obmGQyyb7B6RZuyYPozPW%2FEN5fuQIHPZzBAWjmh7v9HviBBsuLRAipksSYMJOvzcYGOqUBN%2BU%2Bru5gp0Z9yEgoqJmXUqm6tsvWL4nCQybNbsFdncd4gkaWzF6l2XoOrw%2FVKRORS5v4We6fRKb4RcsLRxYHVdlGpBnNB57qp4xFhhhWXRsacwyxJBfSggshoKabERLPQiq1Sc2LcBzrSfTQzjbl12tzNLd1VrgYJ4TSt%2BDUSj7NAo3sIKBW3%2FLT1zFxhYqIO9Me8FXcK%2FFB042BGyZLsBqvHnLd&X-Amz-Signature=dc1108fd606668d2f5cdcc0abe9715a0a0a0143641fcfd184d149924c5de9b39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

