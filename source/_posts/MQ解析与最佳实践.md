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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666DRCXFZN%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDJpH6YcO4FjN6NF7roX7lr5qun4b26VwOG7zsHaZFh1gIhAJn6%2FJFCVcZ1IiWOOm1qA1lt8MV9Tepr7K9wtF1%2BCNKuKv8DCHgQABoMNjM3NDIzMTgzODA1IgypieNxNHSlDbiVb8kq3ANU%2FG8DVc4QkpIJubD7DiQA2fqRzi%2FgWRnMgj%2BgAkGfXlDGELvGdrONh5FPhU3iUAb1p1963ZA8QW0HYX5rAXRMEjKtlEcAC6p9iVaxtUQ%2BfZI8SDzPSLwHpHiwt7SYs3bU6ESMe%2FKKuGMFN%2B3JhJIOqzpymw14f%2BPN9rXzY7bAM339T6dCkgx8n8LEhb%2BQPQTxnD0W1ZNI5Tk1oPGreTnyaeJXUr4a0FHl3MvE%2FE4j4rCAN1g6%2B%2BE4cwYLo8x1Yv7TeapY5XjCiCUsRj%2FjkZQusMB0fTMSz629ZYApL8r6hg7JyXDCzD%2BGrr2S3G8GhzceFSX9lYPsXnui1JoO79toZ%2BWmo%2Bl5YEigVqSSHCQoK2K4zTJIqtIzrwqGh3hKORsj9DyY%2B3xc%2B%2B2EUpOLE6351oBrd2BmHrGqHml4RMJCFOpTxamSN5H%2BksNuyE9JLfs%2BDpRnEM9qR%2F3G%2BNTBYO5IxSFfCAZaeyqrramwsXyc6EbGDibcWIeKRiZwet8CE3ZPz0mqhOH%2FZhEhzEW1vpCQHmjK3AlsPDDqwRlg4NTmvxiTsZjch7X%2Bmo5J3VM8Z7agKqjHzNVivt3Fm6Xv%2FNebH7Ncqf9SEALcy2EH0Z2igCgqfZEhh2d1TA%2FavTCA55jJBjqkAQUox9ditAF7muuqvQsN7LgofX%2BUMu9YGtPQHQuEubsfF45JH9pyaomCfTQXqGHxixNzT%2FQlTgRdDSO91Mh0qMUDZwuwVbirpaeR%2BfGKIXmLTSSPxmWaFTb6k93zeAL3dGLXEZidFL7ef01St4lyivKU4ZDLyfXGCUOveiLNDK2HVIafT2O0Mz41ZvORVQ1PQPoz%2BQ0VacSVRZuyFEtAiKO4qOGY&X-Amz-Signature=75d74e478adf029ded10b546ab37aeb4f828b5c5981f28c7c04e11554d1d9fb2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

