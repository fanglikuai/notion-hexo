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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XSAB4U2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDI9RXJvdszKaeROdc%2FdKaodqh7WdTVEcVggL4MLhIajAIhAOjyWRoVwbQO9I5W5XbYkfV1CZaLZcWJUvbTj2kHFAdwKv8DCFsQABoMNjM3NDIzMTgzODA1IgwK1gUi1d5MOyhPD3cq3AM%2B76NjHRt1I%2BUmcuImEsQ7Zak2GknV7z%2BcV1yfe7fVOJghi9xaoS09%2BfKqSHgrcfrwRc5JSRYH8CaN8a88h7Mr0%2BeOrQV%2FnoXmNLwv%2FNu7v2LjjtdfusqMv02mhANnUcdz7l6ztHQohg%2FZin%2F5IjbOA2Rv%2FYF7VN1szbtEoiBHQmMRSJ8%2BoQI%2FOCh6UduaC%2F6b0vC5iNLhVQ3iRb8067p1jOb0A%2FnfC5fGjnzGIvto%2BW6IlU0OW2faRZaAjBv9YMqNedB%2BUUdUKz97dDySovGOW1BRpGQ2XDRFDygCTMExJEpf2myCbMiO8dZrhyI5NiGkTEJ1rDfaBY9jicfDk9uDHOXDl1G36L4NxWAuhbMGvvwmJB97fPQedf4bap9AcFnu38CfNl6d7qKPycOhlQNcF95CCoqvXxzDbkFh3Hg7xuqqEVs6otnBbkxRLwA3P5Aq9xgr6%2B%2FIijp03%2FP0M4ZebGLaV2wCXA%2FYRxgNxt6I4cuS41Rtxzd0yMRokmlLW1z7CADDew9FUQlUKiaDGRPFjk1OBXZRjpWlguQkM9V9UoydbIFlZsJyPbt76dC%2BD8qeq2xXRHzN6TPmkXhiNKzytsQUvxc84XyohAYFxa2%2FCxT89VI5r9DkS%2FNClzDjk9rIBjqkAcdnXmpY2%2FR5r0zDJtICk1x0s5lHR0AsTHaguFQSjzS32IQ43OclMIbaogezN4gL4%2FhA6pyp4jrsiPTn25ZIX5lUaY4wqme6EVSLi64hKeMjVsSTK3NwTtnrfl5y94w6%2BTG403bS0oaSpUZ011f4yg8nKeHaDGQFY7x%2Fuc%2BaP%2F%2BSZEhPmo5N5WQHrZGN45hri0AjmZ2GdAOUrOvx9S6pEdnBEdmh&X-Amz-Signature=a8df7f00cd1b66ab39bd270208aa9724cda5c42e37330faf6bfd8551d1dd369e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

