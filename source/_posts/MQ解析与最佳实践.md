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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627ZQ3WJV%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBy75OJzvjmYHL%2FHrC3SgzRUGsyF1NQB7iyRI%2BIeXQyFAiEAvA3Lxl6Fc5AFz5x0IprQa%2F8YrqDTR2AffqRNddflcvUq%2FwMIeRAAGgw2Mzc0MjMxODM4MDUiDIdT89ixBqY0%2Bubk7ircAz%2BnwmLAzDlkI4F9GJymiA1F9TJpsvRMUJs0leVv85m5zGnTwUbF%2BClavVHClVv5vhcC5OY6MPFPeXyhWQJQXrIBU36swsmRx0SAz2Kfw6KVewLA3fFMvM4msX9HGHicrFc1Oxcb0ZWlwFgQuU7M%2BZs5wH2uXEQ74Ah3fX6mJQ679aGLG3AGJRuQqyVN4exh7k%2Fy%2BSPnzenTaJc6BrC5opmy%2FsAqoT1yBxkAwYnwi6NgsYnODTJqsYX%2BtZ7JB8F4BQtw5qFEc8EdG9z%2BAoz3GwAhWLuTm%2FEIhgcfsfWGQ1Ttq79S120OlFkF9VEIBAiBI6h1sizwEQA%2FN8uZknAQnKl6xQPN2tzZvwPLJdo1JhTQxkCtW%2FgRl94%2BDweuoAECNeOfr5pb%2Fr2Joqnzly%2FIetImYvrpmveSWI%2FWv50LccCXw0xZV704gUvlMFtl7Bbek1nSuU%2F%2B67Bx3ZvhWtVcHOqrWyd58ZmX921whsEKifv58woh9Gb9VARe0FGH%2FRHG3IR10NqP%2B%2BMXU8UcTE0rqciblD6WwqFaxvWrr3gCbxV2U9PbgeS8bEjLTRl%2FD2FW7iFNmmJrlFQb%2B7xi%2B8LjcQsuPfOZQK4lF%2BYj0l1rBQReRXxwTdqVppgwiYtgMISFv8cGOqUB%2BiOWNho7VgZWKlAjTXrf9Fe75lC9Te6cn2Lvo8GIvJoDaevptoqn1C%2F%2Fw%2FvG5YNJdrVdhKvPD9gyP%2BJBz%2B6ztiBii%2BxMTy8mVY5q3nITbwkEs29tsEcIa0NbzF%2FebqVjCZVh8dmfdSZQ3CYGOGVDNozImavkHhIDTLUfRtNWtxO19sB0TFo0y8ZF6c2PeaCfgJ9u8p7qFXosRsVqEs%2BDTmIAlT%2F7&X-Amz-Signature=70e9e2ba7db73f4a445af2a239874441d3bf52e89e40cbe0866fa2511f834b67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

