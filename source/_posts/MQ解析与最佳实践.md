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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CBMTVW3%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T130054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDv329Ddn6vaJQ%2Fxq4yti9v8WWlQ7L5t1H4HkwvMfl%2F4AiAcSmdPwGNdixSkWERaAtJDr3RfGEzVN5pJKYTHT20PhSqIBAiO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDZtsPwIwCt6nOOJSKtwDxNZJU0P3VUsjS8Oz0HBM5MyAFaFu87qoEFhm35WWXGWSiwlAnRV6fn5v8K225r5d1qObcmCfpt%2B3xMLv7Fu4pOvBlCh0qc7FFpW4f3JAXRi%2F81qnrjUQVRe8xRfBYA8NkqvU0FaIJgUK7CrG%2FF8aM%2BYURBOGxwcv6hJQLdSEVavU2cviJhSEVaDAimYLlLE1WoW3zPQ3ULJMyP9GTsXoJotxOVwDBjlxqYq69wo8Zz3VPa%2Fm5sCil9XWffSIPXNujjcb3syDjoU4SGV9cljZF9hYaYDTfUSASiDMnOnWTA2gx6iCZLQPH%2Fa0ND9QJwPfZjrJcN99w%2FMX9hHtuR2pw7jjnAG5lDOLnLfFFfhJ1IuM8GjMrOp0x%2F5Vsu5kOTpuT6F5HtKoHRKJx4vKLaxcqmswzxtmlLyufWW70wwbZlRrOy38wI0Y2Ae27octnZ8AOKhYPQPRvG9o%2FytiwPZoqlN5bqiAHhKIN%2BD10f1JSyqp0bRFaMQCjTrY%2BMbUUYRiIXS4ZZzmX94SAIq8bClfVshWvkk%2BwrRaXBM0csIYIOs7x7gBb6AUGs9vmni1WnvPKQwXABPicAxF0M5kl8ZmqfnrV2flxcHSFtvqLywp7FeWiIxvn8%2BurZcJCxEwpM%2FDxwY6pgEK9ym4pTETf2GQ3OqbnWeOE74Vky367SY3jZvm2zYDeWCm9DrFldlryNaVaMP3kZNWJ4mnt6%2B2L2HFRFlUtnaI6X1X8abawq1OUZhmeYhds95AW3mUU2LCFn6hYGelmV2JZl34vHRfFZ5CyHe3OmWgr4rx8O9qTUuRV0IfwcnXdkjCQHb6YVqEm4sz55Fa5EaCM1W7kEN3iC6XMiYrd%2B6Z1LH9M8fL&X-Amz-Signature=af262261c305d06b08c83da6badefddc88b93dbd0a8e5225c620762d10e2ddc8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

