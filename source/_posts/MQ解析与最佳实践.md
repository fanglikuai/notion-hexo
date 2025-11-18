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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHNLD5MG%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCDf9DXVXm0Nis2IM%2FU%2BPwanCOJ8b1iOPoMWXv%2Fbpz5xAIga2QizYqBuf5XyZE%2FeVLsPIOPTorr%2FfZS36Ql%2BIS68ukqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQYouPbjm21SP0wAircA388%2FSSLxTyhyv4aLrnwnCW9QL1BaTBY1W%2F%2F4ZVZ8OBEb17yO%2FJzCk7zrmNka5XRacht9YlHNdm2RrfPbYw7YwjlsSFZ0%2FzfUv1agQK0rUmf42mseyhoygS4gdgsnOGLn56mqYTfRSV3zrPLg%2BGlyCclKgjP2RSGRfrz5AYIkvti0qvfmGAo5oMyhuhTmnXfZDO7Cu4eefE4ftg1olWhOmXF0V%2FMuKgtjdp0O9WqYRk%2Bdfx1hqDzXZfnTN9Jw9Ra4KvAZmGBl9re%2FZT2ZhgBC2hwE8ZgvSGu5kD8QxWlKLxKOb1SDCIPMF%2BM5%2Br20Rl2efaab1c18ufuf3HvI1bI7%2Bi3tSIT%2FCWnk68AGZHkHRUS7S2JSeH7SQpM9JLUDFv64Ir3OczU%2BcKB1FbCLPy4hiCiF0ty5rPBCmLF%2FwyHxdK1g5RXjh2hpSRkMHpLua2TQlJqbhM11IrIfhANvqW4U7ZOalSLQZLBEgWqt28bvzZRGypY7TAPOS49ofVuHm0pEAb8z%2FvwlzMx1RL4AVk7X8fW6kPGdY8x9eEzbw2jfpspMUr4ImbhL0YpRsFuiP5ynXxpmvGZ%2BJJ5xFxRvS2%2BSwnIoWpKavLvwpNdOwGUQaZ1UDjFeMwiNEA830hmMP6k8cgGOqUBnm0Ku87FWNDhK3CGZ3DpJxVxLqtk4zRueKziQOJWy%2FbeN265P9IOES8iTbCEkovH0426EUwFiAImfnI0UNDpiScBormu4ZnLzB6CFrotSx2upCN0LV%2FXdrqPHhHaBn%2BJY9DtvQl1Wyj3hoSDaNf%2FM2kS79LTdULd5rnU0J7sGQRH%2FPjQ6c3zq5%2BgQ%2FCXXqWYvhMuq4Ibva6IcytzuEebnHLC8oyy&X-Amz-Signature=02bfb529ac18a88d2da0c56c876ac3bf1984ca7e831224405bc0a23f1e318e7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

