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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JNSEF5J%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T170042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA9WfXara5A6LmqfthNyKy%2F7jvkIItipM5%2FsOXI6xa8jAiEA5K2WnQttw7PrNLhou8OR3zE%2FL3cD4ZLIAyYIsQt7DzYqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJKyl56YneTmGGJfLircA02PNFJiF6ygNoyvVwPKbihx5C%2F4Xw6Wn2nzrb1Gz0eBpjRrl7JHE0WBl%2BmulzLWyIky02HUnh%2FR0nBYqhW9ix6lwYF3RMArKPDr4QA1V17miljRm8SSAlI2c2FEe0vDIngWKs3WQqZanxI2cWKois9OY9iU2ij5dCMBkZRpXrGJp4c8TI%2FnuA52CYD74wIF9SIYed05G7uS3ASTDrVVsOGzjQD2SVecA58KH1yNtNiK0BZF5d4tNxYuQYWJ0TCoRc6z6WmLZwjmYLgGszLuz5TT7Hv4a8vvfSO9ZToY0NoIdviF2%2FYyKGBf2czXnT1Wnt%2BU6vLSPow6N5fcK5OiC%2FT%2F8zZqbJQ1%2BuLD9u7K7bdJztGP4%2B0MNHJaKHDNHNwcCBijFmjnYDoFq2UGpenjnGjEbt1PrciwRQRbiOaPWvuY58ZTfrrEdhWow3%2B%2BHBgCud4JEotjI%2FPkCuKIl2siJ4tdeRQxyll9ZgLvvy4LUgE5lL1I6hQLCELIRvd0w40Zt66SS%2BlS%2FAziAG%2FPEvbqwE%2FaVjl2ZHqblczNVH7iRwTfLOGrVErY7YchSJ2T%2FSu0zvCCYWalAJ2PKC6RDmh68lrYAlobdlwoPy%2BKIVXEJRyesqSYYn1YEpSLaaSkMMCl7cgGOqUBCZYZOXBDJUDIW%2F8rnLq1M47p21diZyih0YesImLJm51WR5%2BCwc8QXtLt0352YxcZtJuizONuoFQcTzr1U91aJDImk9%2B5jrYsrSXqblTPLxZokR4dz4IIiZ6xXbscJZm0qJ3x3LLaN2AiB39eUvYhN1znYG8jQLUUHAfnv0HS06CL4x2Tz8%2FvVTrmy1VRUMSIclxbngdOaAREbBy6k58F9e%2FskwaS&X-Amz-Signature=b50246e4bd4c269d5acc2041a95e0d05a361626b05b2b8fa92408364af70d767&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

