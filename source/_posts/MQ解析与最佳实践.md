---
categories: 整理输出
tags: []
sticky: ''
description: ''
permalink: ''
title: MQ解析与最佳实践
date: '2025-09-04 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ITFFWAR%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T132915Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHkpcQSAPk6LhitzBPutn%2F0nnIF58U0qjORCcFUHi4DvAiAf0XB9CdvQh5f3srDRG40hmcoHGRCdIV%2BetXUTZ7UyOSr%2FAwheEAAaDDYzNzQyMzE4MzgwNSIM6N%2FS437X1sSB8ZhhKtwDw%2BO41Ta4S%2FtccIpLgWv%2BafiL1Fs38WFh99ICyS9vLClLvGo3%2FuxKHUrhtwKHHnS3fmQU%2F%2B3jT%2Fsa9UQf%2BpNbck1HqQ862OUu8i0qignWYndwO8cejkqUrI1b8bDoKd2YgSl6huxCOt4qWvPSsIbYcHq9CgBxBUX4OVLtZSKwqcYkDrmcsYxUivEHEhjJrDkfgZgYOhvAe6swuEksc15oRLb56vlimZTQ%2F%2BsPVsXIcxV1PMlRfOigxfTfir0kvzpfRaHWPKbRx9kCTpTr8WpsHv9WxfpmWcDphnXICimxVS7CQMdJ3zLyhT3a67pm9abfiwCZlCs6TFC%2Fdwv%2FK8O5NIwaJ4M79lhy%2BPwEIEKX58O8Ppl8MRZBcACWrW3ZTmfU%2B8oqiXLfIn4nQLLNrtwUoDLzAVWrEfZ3RhYxOcAQUQcH00RHUuTwWabvvUvTiKLPHeuk4hSkUCl2XTu5hn7en9okqLLTHtrmMmbIg2WRPIgc5RYVEAulAvu1IhrQpw4UlWemhVcBIFjPad7Bi2Cdl%2B15%2FbPsPcgPCWup%2BnqJ%2BZJXFFSWfAaMKo%2F2dBN8eIgZZ26h%2BThpuXlYbgvW5Tvbd55M4DaUL%2FbKJkUBcLPrkV8Vn9NrfLPANzoSxcUw7vCaxgY6pgFL6S2MbXuEVybam7Z78bye%2FlQebglewD9dpNj3KQzt5F3FvDFVSl%2BNxWrxKNZE0pCvGsbs00pLSkSGJFe6Dej4uTNw%2BTw79kyXjXbqMiyqZSZ65H6%2Bm%2B2xdSUra7Y0%2FdEZZZdK4SSb%2FbCcXy2sSBQgHtlsLoHxsEy79X6L92zXmchyHx%2BP6GXJQX1zQKN3YUZbCb6OyfiQ%2BZfOcB%2B1QFHNtgP5VOZU&X-Amz-Signature=6325376e9a7c11d9814971a0457541bef759c2609939ade9ffcda55b3395f9ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:24:00'
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

