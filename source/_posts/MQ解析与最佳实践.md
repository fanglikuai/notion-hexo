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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVODHFDA%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJIMEYCIQDJFbq6J%2Fr3wT1Lw7fzamNUwlgLEeQ3RMYKuINnLEh0tAIhAJxykXW9qi0RY5d6LaMRXZCzvgcfmUhzIKqld7b9Cua0KogECOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzCTFqXHThoJEG1F3gq3AMzdBmKf6ZJkx%2FCDhoCrPIX8BHctRJLpiUkNp0aGTrtqXu7zLR8gzkuyCFW%2Bg%2FmdL0rhD0lMWQtjvjrclVo66mazuOa%2FnAMftfgPpW48IWEuMGBpbS%2FczjV8zfoAri5jmoLOCFbWHJItRT5y%2B2ioQNlxfXJOlwsTP1t892I61Fzc1PIlcUq2%2F%2F0hihLrJt2%2BOO3Oe44gsqivNdlSxKy1WFtX8tsm8kSQzKGuuhrPSfA9gg4P9rV3wznT90jbABNTeREwoWgVTEORpiwHNBBOUfbvECZJ7lw%2B568EgQAUQ%2BRqKjb5JjL3XpC%2F8QvnvTSvBHApcf3i%2F3zK7xE02NWLEw2biO5L%2B1YT%2B7pXczJd6TqTcseT%2FXTDlD6w8UOA51evMjzandRM7eV4XnW5Ixmjzs4usdFxTZIpBGLrtOb6JeAK0UBinlOZB%2BmTkFyw9DsGbPGbL718nee3WhM7GUID%2FgQbXwowLJSKlZq%2BKJgNJhzxd23WV6dfwoNesu0P8JX9c33QR0C5iDk9JErqOykS6pQptjWjyYHgkM5WFAeMY%2BrKB1n%2FKydvUCtl%2Fvs5BBE7b3AqCHEhxH6VZFlURImpA9X9SLN48gpjRW4QNQEDTqkLgmzWd0HglLGOf29tzCJnKLHBjqkAQNocUfKdMu2SBSS06G7KGP7Me5zNVPh1FEMsomV%2FlUcKMKvU81U8pFJrNU4nf3LbvRqONwoq9bn3NLP1NXxqhixw5ItwEVFMgO9sbxuRbl4dVJg3XAQ3zN2SzUo%2FaGY%2BY1%2B04b15oN1JUTFSMl5htwGSRrlNDdJ1JjnHsY%2BJge8dy2p3NXd1C9%2BOa6vBx9lE6w6tgqi7JTsLLrlajBuOoGzsfGW&X-Amz-Signature=fd5206c243259e1fe0725810379aa9318f2ef4f234905b8201d55e1bd6f8281b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

