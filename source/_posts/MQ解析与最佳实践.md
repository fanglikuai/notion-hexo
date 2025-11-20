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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UI6FGHQQ%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJHMEUCIQCbSozs9Z6IwX2Fn%2FROK2e6dFMBUp4Dwop3Fl8gMBwC1wIgL%2BBX4JC%2FTfCCvmTmQqr%2BtmWOnPE5alNtQmU8rakk7poqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAAiwrVW5OssATMhqircA37cSch2YmflXu7budJAj7epq1KYAfYKdTIMd5eRvY8PTnV55Vov9McOi%2FvwbVXjUuCZQn%2Fj1E8O8ZqKeYNeZ91sAgo9qUbA77NGfs1r5nml4PuNFxM6FE9M10M0%2BKjAJwToY0pGl7uNIoh9bBBxWSEfS23cyJxygXSczSv%2B9ginF0VfBleCxvYpByZnupYyw5%2F%2FQxvmLpUbTZTkJfPZ91dlV5UakPGvYykglpU8FxoisKSkz18XciUE%2Bsp8RHCcrJeZFhFW5gMz%2BQgBHmp6qTeJdqTA1peSEMNUoUwXGLOjV4%2Fn3kpTFx2OgvKb4QjomD%2BzUTbcdcniF%2BzmEOug3cLMe3sqkhMxg9VWDFo6FYVQUSgk3awD6WCSqLHgpQwmlfbdHtuyoDSJ0jR5hTnJFQSlmxLOZkh5JQZ7naxp4vorn66kwl0Hrh5b0OndE2iaKmvnik1lD7w5U0aLKjoSwyWzybIpjSrNj%2FJoyP0%2BYU3MFqcozv9aQN7G44Tjv5L4zLU1suEv5XB5gahjrZ5Pg%2FJKWSV9pM2gZpQdiPQkKvrjpp9HY%2Bo89chyuE8U0iWtFzISHJMxf33We0jgEVcchuBbxduD4UUdLqxYCzEp1suXIjRUoT5Pp%2FpmdTfAMOv%2B%2FcgGOqUBUTyyJy%2FSRVHp4ScrtEMzt8pr%2FAPBJ8pUgnkU%2F1Wdh8t2JFdawqqLqsKwyYC0FKZayTHXvGiso2yAtzbzlvN80PBfG5Einnv0GEJLZJ94bIuO6w9qi81fcjP%2FekxQwJFp7y8zjbZUG9xQilNqmc5By%2FMXTdHIUK3TPBYe6zaVKtpsbOx7SxDtSYYgQgo7wnZwYDI9CApB4VaJO9qkqTuN4hDRJrZx&X-Amz-Signature=9e7cff0823f4ee0f606f4d6b8921abbecaddaa2a13c5d2eb8ef4d13f6b055236&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

