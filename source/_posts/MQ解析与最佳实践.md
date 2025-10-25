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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662U4GCPUB%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaOQmvEyFFgeX2U7xLDsOqgBWRLdL9xEFXbNYUxOnVsgIgdLktrfJNk1hakTDuI%2F1ruj8v3cdOZ4dcIb7wd6aj4Hkq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDCl9o%2BUQl%2BXRVy3pKSrcAwLE%2Fw%2BU61jTQ61EPSPpj%2BUy4tbAc%2B4Mfnbc7DewQqWg9b08jYTk99xki4Slx1mQ1ojX2TPoILuosezGXNCqZVAVcvxuheTjWAoHm3yB4GjVk1EJL1OnH028TDBpjIxyQaGvoQriGQXiZz2vicZRW6OD6pKQEKU3SHBIF%2B4AC8Jov8mM96alV4eg4SkAFygtcwlzliL1RCw8ktBcZK6cwUu4LZPORi9f9PSEr0lMkP00LMCt25qExaE71F%2FfqTc87vLVNYj4hslqQ6OKPS1uy6tDQf6mSwkYMKR8mMroV6x%2BoJdYzZ01kORFRY33rrnjo%2FTXcyMCjq94cOPEQFLUXnAGKvGkwSu0Ax9ImSlA80dc9FMDGggM%2F6%2BSBCvO9%2Fcv8y%2FvOryfKHOlRVDQofW6HGj9%2FN4o737FHvDXSfzhTdSieUQdiWmrLjcDBGm%2B9z%2BywDR%2FiGoO19r1x2vrDcFZG3%2FCLK99HgDdDhgkjRb2t5H6mI9cWJkLgjDcNbANi%2BnCqXU78DIEPdK9NIb9WpqM9JeajTxW8hTMZl4%2FspjCp5g%2FR78aSLXTKHWYU9TD6k5eaCXgknEMmIln0mzUTNzFxJlOMA%2BEg4LEH3UCA%2B3m76N9RKITARSpx8WzDYq8MLfq8ccGOqUBlGVvXhC2%2FgOxwJFZkKXMVM2osj1943HiTC%2BJ29DVJkIsGAUZhA1Fz9TRSGuhXuwhXDUAvI840aqfMaOEyi1AQu%2F8ymvKzX7%2BrwMGQKpR%2BvapCYl8aSeKzDjd1cIb5sKCUCf8iQ3lbmHXjim5FQwDoo7XFCoy%2FQdC2YZLUHKwy0K1NBenH960Eejm57Ux6ZPA5gd%2FfftDnklLDBLtMLgNUtguTA4D&X-Amz-Signature=f82440f391f3aa87f73ac7d5f9977a809244065c2e1b86101492f6a14e9a0803&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

