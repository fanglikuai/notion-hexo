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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZDJFYD5%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFqDXs3Ujis4sXtAD26h9gWdIzsfvU1laU%2FiDDVWTHakAiEA3RyXafnGVnPUurrwzDsoQ7vcHFWKILbvE2Xgu5WyKkoqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKmL9JFGjEOyp7TwjSrcAwJ6whGZ5Nb2Q%2BjxNvF3SH0tXaDPG6oK%2BepvJOY%2FWpL3KSqTBYspiG8bjgFmhEhI74hbT2AdZn3SiFIAPe4bLbuGeWkNLR8Hu1OIhGGrjNjEo5ZM%2FmTqyrvhC9lQvrM9%2BpW8hbEW23LVWF%2Ff1VhSyj5lpSHgZP2Bjm3IZW6rptgxPQHxx1LqntkR56TS7rvlNRjjQlwxuKd8d1IL6LouX5Gz%2BO4dJzNU%2BKc8bO%2FORBB3zOQq59ma9tUJH4ir9xHxUmUxMtZt6Kp64m8p7a5%2FF9U0PAi08KuUOXTEy9blyy6ciSePPIi14nabB%2F6HeEyWYgcDn9%2FLllRJopxNEvBGRJDHB9uZoyKQK7LPHTGl7SEvtX%2BcgGOQsn0Kx5f0EYVHrD%2FORNYzE%2Bna%2BryW3x292SA0rUCO%2BmWIfgVUcLPuJMtT6nwIpyO02rA9elD4AMHe8wPSCSCxb7PEQm7KkNvelF9IHTkXraNdKC17ZwaqapfkkIRpLWhRm8OgM1FOjugsMKZCYAi7Djl1E0INFDDlaQAWgCULpS0rUR2Mkf5es2RCrsQG%2Brimu4Ph5XEHetUHpUCxQhpgridzWakm%2FDrj2q72U%2FU2MDCDN74uCFQG%2F0s1LS0tOQqNJbV15c6oMJbngMgGOqUBx7dUhBEdO%2F3Cg7RmJw08APL4B8sb9hYOTgIx%2FcN3C3mdYL3qlHq3%2FhH7cgy2pi07Z5eAcSqiUUdsRp0q7pgIJ6JC1GZHRiEt5Yy9GV5uk0NTAqHdusLMkYs5%2F4FlMzTZepo0bEC%2BdFGZDwhNY4yvgaVMWEAF4ZK7mGx3r4f%2FovWOd7KThvzchcY1RgvWHmT4AVU0COXJDX6Ne7Isz3aoCimkPkuF&X-Amz-Signature=35a5e18b46791b131aa2f04209465b5da93d39ca55af51bc30fada4ffb815ce0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

