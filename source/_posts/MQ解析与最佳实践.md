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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WFQGH3Y6%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJIMEYCIQCkhnhxATd99Kz256e9FfQh8Djc3E6fUi4NIKpE%2B5JgFQIhAMef5be1jrKZb%2BMK%2FZDWh%2FxhX8F3tvNhjsWZRCaVemzCKv8DCAwQABoMNjM3NDIzMTgzODA1IgwiaCB9uDLmtVmhoyYq3APmob1sB6GEbyqY%2BqlaIKtYP2FpEF0%2FE5Q1xl2G4HjIqTOEi5BiudNYE8VERv5lNY4tuh6IpPctjijCeSm%2BPGeCVmadBOdSirciXhNXFQWPdOezrJSLNAc2IlttiCC06HBO5ajk9ggN4FbLJqD9BjS%2FvKdd12KEcw0wL93s2r6soXz6oJsRJV5rHAGQ%2BKrDRwJOna7DuKwu0ccwVVMtyG2u6rmUGC5sZYBQeeMGAL97A2MdU%2BM%2F0GCK2R77TobduYxkoy7ChNecAbu%2BB6OQBJAO%2FNuC4TQWJjz6K%2FSiElrr8ZNScK2T16pPccRqfY5WmPafwUMWZHEmLASXzJDfGl1BoWt218lk2PeeofJDcQ0ldbkWu2ve5uzjyWz%2B%2FQe%2BNoItVJYY0l6MbOAAzl%2FVHK8cEUEkG%2BuIfRRdJScNyTxKX0ukYoaaK8iY1eYApHabvw9bZXAxqNkPGpgM0quLjZYKx4urY3%2F%2F6436TgxGTGs7PQpMBjYgmos8vOyJ3T%2FhqVZQZPAillqiYjx7lDUyFuH2Hew6vPUyslIAphR%2F7eLz6HQ%2F1ikCjnBN%2Btw%2FjTyoDfM0s41CSE0wSQY8GpFi%2FdJ4gByoZm5o4TwaQtJ2deTcy39qrwg95ko4ePBIgzCqloHJBjqkAQ1tz9fACMvB3SPbqb8oEFk3nxNnM9q32MF1PsyE%2B21TZ9GcM%2Bh7AYxX1DLC7OmQ%2Fwl3LDlrGlaJ%2BNvPQjw29xASiPnh50qn4D%2Fhs3Ihq1Qdt0dH0l%2ByUys6GLJKsA4wodavwgiNrcmAVVi0y8Qc%2FlaaYIp0q5s8hv2i5ZTpCoYmTDknkRd7uRd9%2FYfpOH9M%2B5LZpXEI24pD40QUTuP7iNqXqI3t&X-Amz-Signature=579c07e243bab7a25a9cc297453fc05be60cb037771a00fa71e2aa02d0055578&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

