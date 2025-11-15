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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OUZWBMK%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICriY0rbtKxyZ1NQ29li6XiUPkqcmQ2PP0mtJAohNEQxAiEAgRy6IqPPr4zlgx0P1czxxXq1WjgytL%2FyMqqstxQyskUqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDW5xxxzCxz30Zt6GyrcA1LSulyZdw2feCyGtU2OD1wr6OhZFeLaXwTOeu4qaEB7%2FG%2Fxz0apfDDsd0we8xV1OhgKABnnbE2YVuTIrVeudvs6J%2BbM5XytMGynsY7wk8Eo6AXzgr%2B2uRvjUSIHGi9P0GBL8Cbtggi4zrHPgSKZgd6uYiYQes4z2Sc5MPqsF3c1bz1pd69LYDUO7f0rtULf6jGQ%2BQz2fPu9exiU%2F1DCkLGTfV0nKZZcM1AfUwodGZYXhTweU%2BJucNCsrG1d7gv2h1WENkzxAXUZI1PGfAbu9nqE9VPCApRhV9qitOFQPXu%2BdvzwYMBp1xWgKODoJftkKLNH%2F7Q2gDpeDElo6NTtUDDEodqkOFWxDMly7EW9kBUri0kG0%2FBv6NlptDP8QlY4HJNsdrKz2RmuKGpi1cxcP4pnSlE3%2B6aOb%2BpM8ZLNn%2BptwtOpG0tcxx%2BAqfkeDJVOET%2F3uaN%2B7YPGvvM0jcf7CjnWrRf9%2FCjqIidNy6OsSLYJgYjnJg0cUxTMECbo5ZE0wY1er9oDTad6ACGe8mqJRBWVKoRM7QEjBg93gJU1aDqXDb0N3VAjtwRtRcdiDhBaMbJ4OvrmpVTN7rKiJXRfIbmTG77wbDxs9VsLhL7n4e%2FLPxGNCcKL8V3k0BhtMITC48gGOqUB42mEvwolD7e0owwqmnFVLyskrFroAEqWCYogKteuQuRIADSi9nGoVc9%2BQZpNLCRQhkCPUU5eZ69UoPctbaCk6%2FfNYBz4CJSciSzlZg%2B%2FW8wNZxp%2BlQx4MUlzlQ6kkAQQsUbu5bH%2BcAuw9F0lc4wlzZcXvBca%2BuA5wDGjqoqTWbkxtFscrjTcI2scFUEvJ1AXVNjwU%2BPiGRr7RQAa3R9IQoFV7zzU&X-Amz-Signature=aa73f19da4f7b92d6b8919e68a276dad1eeaebd67328a59805f13a6aee19eac0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

