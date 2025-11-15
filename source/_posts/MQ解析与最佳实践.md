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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3J7NQX6%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUGI9eyciCX65Yo1hCDJcBq2FQ3txG53RgrVfBDA6a4gIhAJCE6dWtGX%2FuV4O1Lpu%2Bg63gMcHVp4adXCXOJCUx2gJhKogECID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzSo%2BM6imZgunhEvXoq3APezAYLfpRyirlQamFyLJkbWMKKih8EIiPzc42TxNB6IC2A1lMntK%2FG%2BuM0skzlzRQZP9%2BAWBZAntJ8BicR92n8vFJQOeQu0ve38ZbfH4t%2Bi7A6L2fJCRkRbcnqQv7jrrC3wbp1s6cu4wCpMvAZJGHOe%2Fm4swNNmPOOE1H57wS8uBiQJ4Av6o7O%2FFSGo0bP%2FXeGB1wkZCcN6JOe30clZI9pbRVAJIl1D5CIKAAYeEzTNPGxTMceq0CH4lQR9q1GTUvR8J8MRz9FNw4z%2B04QTBbVzhBrr8jkDDJ6uUqOPba2SVHoYv7QY%2BM3xqyIX8H59SzBvKY3iXKQGGVUK6WPd8aAzCM%2F8RVblpMDB%2B%2Bggr%2Bw2b%2BOY4hI8P9akuwRSs4W1%2B9X6sYlPUISiC2Lfsj1Izie%2BOKKtzepG9QqkSIUgnBP1%2FkTnck63SdalvPwFtTK2E%2Bkn3u%2BibsUm9K%2FCk0Lr%2BAIEKcAvl6L6uZfoY3O1qRR14qy1AJiwxk1cT5S1jPmIMqS5eHAwTTrHij%2B7od58lCFVOkAJ%2Fw62Qmi%2FBxnriDYVZwy8w8qid04va4l6LLsGaGe%2BkZdTdSsnx5yBuY%2BCqiJdQ%2BzgjBKPUd%2BJnyP2QAMpPeBN6IwJLrVOIroRTD8ouLIBjqkAYJsLkAywu0v81vf8V6RE8b1P3KnUmbMbx58ORvJqbIgjmGHE7d1TZI9s1KrdJra9JwqUS1CCHQncuVqPIqWAltRauh7bXrvzj3y84zE2WiNQ0fY9TYoxIKoO5jnd0QjonCDZwr6ft4dyh4UHxMgDz1Q3k1IeSTZSwztWVq5pGa6w0njAb9CXPXFMh%2B7LnN%2Fch9W8NWdjakgw9T6S%2Bd0Ql1to0UW&X-Amz-Signature=da89a4118abdc3ac5f98475b459845ecf3a90ebdcfc5438a7b0c84ca7babadeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

