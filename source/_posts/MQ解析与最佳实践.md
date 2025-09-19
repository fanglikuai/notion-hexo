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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4TZE6XR%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T050113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCICTRB7Dr8Ns8txzPUMvJttQ9ZTEkvX5cJrr2kROtlGl8AiEAzdsRU1LQbDIAWcS63qE%2BfCZCSZE1Tm2oOswYrozsthYqiAQIzf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOtlpy4184MhVgZ0yrcA4yuhv8en6MEnrY5kTJzHsp8lbdwZAwr%2FDiBjN%2Bd86fui1yP5AHM%2Bw5XjgnNb289k32lgQZ1DcQZ8rCMcqcHb2z%2FCaF56VTagTxLdrg85sg%2FkRS9oTeEXLheZAr3S8UzyuK9uYwAoR7dp76h20mgvgoRSbUJ6Iv5BwlLvIyEhdK42BaIqbhCJrM5ROj7HXOPZ9pVbP8rD8pKigSbzJIi6RYTZDcXC578OS0%2BBZx07kwZBk0ayOXUADrrwPjBpizg1QEAfX%2FFcn5nQTCuKEzIBSFktKnXd1KUQa8yrK0eB2gkJbkXRj9S84GwpWVzZ3f1TS6935T8X9Z%2BhUZkzYBcpG3diqeoxD%2BBmCH8Y2ogQXnfsATL50irTtluMmu5JlCW26%2BLbbsPnRnUTY4sOUi3B5UopLDTiayFsrr%2BPZkqO8YkjN3wpQ3%2BwZvY%2FGcQVZZot41kXLaXAJB4nATZdxiXIAaYssc81i0JMTMr3jRbl%2BjqApBDWmVSwbCZijamOXnOrg%2BkNCYaavhKw5muBdvwNZp%2FY7EXxPEffjotVXbaKVxNkls70Reu3Ba1%2BxnKFKenI221RVKXRiMTx8TLwdtFZKGWGVuO44ZVCrv7KPpn5JGoPKxZfdZqts%2B2Kk6TMPC1s8YGOqUBX1IdPBO0hYUKBVBI9KfPJ%2B8KV4oI%2F2lKE2LITe3ihGAzVb1777PZi0z51dOZokaYPl2B4ogPw12ilGp5pmar9vYc%2B7WupSQTYzxbhskaWIoOfTR3hSBjDqMcDbzdeccyLTBkqdBRxJ618ZRCUEEEdOsfzqXy58j%2BLHfHZ05EbCf35GS8Q5RG4kJUAFvLSYBv23JC5OKOkcLShprUezsNKBGbt536&X-Amz-Signature=4beb493a05080cc6b5efbc4d380d9c04c4bee8e346b3bf685ca88b4adf935c60&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

