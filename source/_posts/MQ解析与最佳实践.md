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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TF7VKP7R%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCIHKjEIQJOzru%2BQYKd%2BAkEoMWa9gqnlyUgZofdsszFBUYAiEA0noZ%2FzQScdM1qPKITCB5lOWEBnMF%2B3Rd5mbNGXfRB6IqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOFT11sRWpfZJ%2BlESrcA%2Blw%2FX0NGJMnWVjPYKVsMWl8gImSJ9X7EV90EF2WYnDtpdxDrz5tI28iAawVP4BraJYV%2BKmtM7lK%2Fc9yxbIovtebMjakkb5iFmk36CyPEsnn3j4I%2By%2Fu0zTafZPFtzoZa0RXASZu%2FvPwZdk7TjTJNCBjxY57rT46WuUh2EwQPzrirAQprjQKYpxSFzXF2fE%2FubYP4l6%2F2PG5yAOYSalZeuhsStj378IpyTX2MnHIrQKjnh%2Ba5qIdeqSNr8GRkX7R%2B9bP7v6bRL0p8BiuYQABfvHowhP1uHh3u32NNNSNNinZjW1X0ZoJiGV%2FLrMEKgll7VOznM3O%2FuzcFY7orSZeM7nCCKSC4wwROHO%2FQuVcPzVu3UQOjwcNVQaw%2FrNeFeZotzodw1HFlpSMV9Ty4WiQ7uMxOlhu6IEnfr0078EV1MZx4qflfGj9eUgiKtJhbKDKNRhCsEo41%2FDfncF5MyyxutMJewttWUagZ6jUUSkuhBCWbAjGG1QCDxy00HoBwsseDl2zAxLTMR%2FHMXDE%2F%2FWzMi8%2BWiPMWsaYqjlil8Z4jrqdpl6kMh%2Bz6FrFtTF6L0NjUsIi%2F2673HTf3LZuU%2FoNqx6HTdsutHbZyyWaxF148rp3breg62RkPbV7gdprMMbIscYGOqUBfqcGKwUlF5G0eUey58X%2FxE6pMOvdwiz7b2fe8giz4QEHfd2M4g%2FMDFyRVsfSuXcMm2QazkYmMDWqDtlCWVdLqcIEE5tAv%2Bk1MFEU8npAixJerLI4IGgKN%2B6EL6jBtWMRUF2Eh3%2F9Hy1auiFXkHv2Rqzal02QOnH2wgnd8wEkZJczJa1I7E7DD3qzD4uG%2BXDIF8cwdHm%2BWfiDR5vLR%2FnlybP7Taq%2F&X-Amz-Signature=fa592b31922a9378a9695c05aa57a957950933550494d95225ff0fb35c8c14ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

