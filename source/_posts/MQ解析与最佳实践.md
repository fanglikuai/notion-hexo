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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635ZPRZM2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T180115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDTlIljWGm82KvvYZu3XVPf%2FOkfpSBbGa9YEOIrGvn%2BxwIhAK9oWhxzvZXpR%2FVC2ZeuS%2Bqj%2FgrzmBIzu%2FG6pIqBAZ30Kv8DCGsQABoMNjM3NDIzMTgzODA1IgyMlTA3Dpz9c2Sz8FYq3AM%2BwJPRqiy%2F4pfQF1EHqy0eoSi8VcRan5gmcPGVwYPcDoE%2FGejf%2BqhFeJF3wM5g3DGUvr8ZojDYSpFlMvyeqN8EkiMFV6okkvUgOUdpJdi0C3T9sWTBoKeDERnlRM55xnMxrD%2BrurnzTfZ1sPaIZnoCicPnx6A9FKH8X2W3BLt%2FNr82gtDrJGyfV7l8rK%2Bmy9VLPFowf78iM8n45PucQbdzGeI7%2FYoAYRer0AOd%2Fv%2FY8ksKrPHoG5PNZc6txqc6dX%2FU92iYst1WN5EI0lGYWENR%2BWrbTtun1TDYVdEpfyj7DEs93bpVpwqN7%2B3NYyVEXOEXM3MBnUgreoEHh9e%2FT8uGVuk4zXlsDg%2FQ4luWnkjCx06eWVaX9z%2BPNrU8r1SoKGqhhWAX8zC9L0x1hLSzj9%2BTTcVi22TqjcLCj4NlSAlOB6066dzi7kgiYSvm9idiR%2B3UJdZtBPGsQkENaetorMGSXhJVtOXeAAV%2F%2B6wgfFKGzSQ6Ee1e4WHkqF7JlFbgX6qtdFLzmPOpn0uZ9awCPbf6xZexrxje3ovR5o1J0PMZoxHpDV2FbR82PP1ms%2F573akk%2ByC%2FdBkRVx8bLVcBntvh%2BMLBZHmI0%2FG4SeangHX6TwN0ZaOM660qmfK0UzCS0t3IBjqkATHn68RToIySPMgrjfHwVBltM2qRch1O%2FS85UUi4PG149nCrcHDr6c2EWLcLfVbhnMyQ4EeLd%2BQjm5evxWVJ%2Bz8BS8oEjFxTNhInzJcYJVqkPOCcUG2mzcZiNHahLXZGfLoeYhaPwRpGhidnDFgw71u6Q4Ed4N6tZnz7NKjeBrJO49GhRo4boIP18ExI8YDT74NV2Xd0y9CbhsF%2BTtKxOmgLGafh&X-Amz-Signature=a5b0b8f52ecc1964c3f60d4f3811ca4b53ac8751be5e0d042ea3b7cbbef67017&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

