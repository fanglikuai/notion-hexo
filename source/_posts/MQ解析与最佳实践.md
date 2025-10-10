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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TAOSTC5Q%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCIQC0PoOTundYwI5qc0d%2B1zBcFH0ci4XpMnpmceV8%2Bbp0iAIgXpbZMYwa%2BLUD0903kOO3BvCZQz0bQZljD3XiqJHEs7MqiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL%2Fj1duEUBTypg1LuircA9U6LlXPz%2B6E8A7edILlvXMwG2AcSoFbEKKWYRhubwhNyBFBx%2BndWTATRARZ0zHHv5lkm1OJbRK%2FhK9H9pgB5aB0EfjGYyG%2FOOdzq8TOK55gqpyusadFwlSz0EGtymA5rIuvEMKrL77Vr%2BhtG9RISvG6bm6DUuaNUBezS142SbVmUmKUyyRfozUeJ4ObyuRI%2FW%2BIw8eg%2Fgu1GzQKgC5yDYamwJstDzvfSAP8ji9DrFjw5mJU6T1lYCvqpjWCYjPNCWD80icq7KpAm7OI%2BtbzM6RJstAN%2FHz8Ua6RB%2F4FeKPRvPjo0ErxsJweFAFvFDTOwHHJM3n7jE8EylE41VnRwZLaNSrfUClyp%2B%2B%2BZIZ0JDlyUyMCTWqr%2F6oVtnhmiPfblfIfsXNDjBszgTsCoa83xnWe9MEzeufQbPdmtsErq8RNr3dz4Nf58aoDCHT0u6d1e1pEtmWofGcLAt3tRt80dyRY1Y5Rh3kQBqp%2FN%2BaxKLDvbpfO0KNfQXZYIqZEF64aY59OdrYnHUh7oMIZxijc5CiIjKcYFaHRwnIt4HmzdRBpDcMrlUtW0ZtRXFzkvSkRs412UPlGcIU0O9KAMQxmXIjWpz9ZBR4zxJJ69nnxfZbMHF0m8aTszR4P%2FEAlMJGApscGOqUBgyV4UTdAPDys8NIedTDaeOzGeCMUVnPzjQ5HRZQvnTTeaHn9IqXWRHxZE7ICkOo4CWiQOgjt9Afp7nVxw3dgPUPPXtU8B8qMdyuTFjToBXFSKLcpDA2oiV6zORoTYpgzt65WLdcXCjo%2FJgUAQtdo1KjTCf0080TXAxfDDxVkBg2uIVdemoQcMuIxpprnXqZfvDyB08c5%2BOpLoFRU0b9j1VuwNE8m&X-Amz-Signature=4a19aad945eadef6b8a6bdbc144f8a67ecca0d07379a9aa538cd38683c731247&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

