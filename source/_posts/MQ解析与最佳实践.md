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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVGT5AQD%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcZDoAkkDi8YifUHBxCNMYA%2BjkT8YBSdQtAzIOUVOXNgIgJmmWi%2BsYCsa3Mle5u0y1UyTGQIY5QNkKLT1h3imM1g8q%2FwMIfBAAGgw2Mzc0MjMxODM4MDUiDE2%2BFL%2FQXxSB5UB6gyrcAwqDObrFnePvKqHiY96b2MC865udqCl3ldDmyImdhEpcirGgEfuBIqi5QAxjqnROWkohiM4DvfjLoKlbm0%2F%2BkQp9D1LuDkQKi7H9ORiaeRDhL5U3xAtjCAHgjVWY9vxKnSJVXYey0FCDYo194kO95jlFN1hjCoF8%2F0J%2B3CENhAWTMjjJPw6gls9HhNTS5g9ndIxeORsdNqR6rwOfW9DRGvRuYmvE5264QJEhIsK%2BC%2B5ELLWPtV5U76JR5dqnaGFWhPf1jJsxXsXGqTJ6GP2mfkVIXyq2TBBjZy4jqKpsHNeA5nfVJ2RY5%2FvefqweXjHxhY%2FDvsikjOkm%2B5x7iNMDiTu3WyWBXqa05LEfqHKpTuN3RnDdABJU3FDV95M3kQ33u%2Bfcg1R4%2BUbmQpGg%2FeSAH3Ht1A%2F92nND%2FnniLMgdGpHY3B5ehnmqdyECWNrVEdTc0tnMZXtb%2BU6zQbF%2BkFaL14fVMvOGla4xnNghgEAJV%2FUF%2BtANMzKAWL6ZBedlnsYEgzxEx1nL4Tr19l%2FL1x55%2F2u0fzYwKxfsDANsCJ1P6nRQtvQNKPc9g2tnmM9XsaVv8eKaxXZaUC5g9eH%2BAr5L3EeEGVp3KBcFOxDued2oVSWuAMfz5ZjJkK2HUWswMJqiqcgGOqUB3C5nfJiyXTuWZI9ndUuK42vlKbiWQ%2Beh%2F9ls8yaH1pN3feyIYmgzOKwO%2BS2F%2FrSsD%2BakELk0CKnXS315Gm8b%2BFt9ZS7OT%2BHJlXCYJgtZTXXvbZ0r%2B5caIG6m740YKB9l3QK6ftKskqn2Btu%2FqgLrBSxqGtnfRPezy1p4zQpUzjIMvbFGmNeEByC378HzcqlIR1bNS6JArpBULzJ7UZTmobplkTgF&X-Amz-Signature=aac0b47b541ae509954b873993fe98040ed10f5109860bec16d2eaddb2c88b5f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

