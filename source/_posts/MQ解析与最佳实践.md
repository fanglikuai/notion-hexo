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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2A6VIL2%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEOT%2F3DFFVPfehQjbehqAImy3lXA96%2BIg%2Fo5v809fT6RAiBxST1Qc%2FYfrJtPU1ZVnm54AapD1L4cO39Nc%2BXC1EsRiir%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMJDOqndOVNNXTBZYKKtwDuteArNyQp1HqY6%2BVziO6l%2Bu725DqQcfW00GOHfNdviqmOmb9ItkzNNipelPRHwWBCyLi1P8jOMgaLw%2B0Dt4MRKJRtPFV7oDynuDV5E0qQAdT0sBgNNRmP%2FkpkNnoJKCCbvHpW0cotnk4DNLeFA52HOBh64IrBb%2FnM8GE%2F19gK%2BMVByIh6TvQ1A%2F6o0DLH7oE%2BUTt10TxJJOEijsaQ38hNsB%2B%2FKkbZD88M3HeVmetodLClEVuByuMYbgo0wwK%2FuFMxyl7wJpLfwE8WG4peCPKL7JewE1WHwhwZUmQf8%2FxLtmGWMBkY2L0HN%2FmjD%2BoomHyxyakyXSrOiTefjtGW%2B9Sr61tRJlHE%2F%2F4Gtxdb9A5%2FY7M7EwxG4%2Bc5mRKf8MbIVXyF7aN1VqiOLonoEnA%2B5HvmfkJlSTwfmfvoK5ie3A%2FFEVbL7rUfEtrQDw0onJE37shqCCxs61gw450l%2BjnrVTviZiSPxaOEcc%2Bpb3nbCusG7qiNCtXk4ne6zMFEZEUKvD1awLKwN8AlhfzuuOJKfcgaP8XvycmGDsXuYJMBdGCO7UpiajjDCmHjNbQDFZuu9BQnmm%2FXtBfpturGr8KWSuOFaNhW9Va65MzVegciOOJGEp2MyvXocgaSTG9liIw97STyQY6pgE9YIuETKfi3o7WeCMDDJWv0RR42%2BITV7tOmhlkPF8X0d9YhgmVFgwd%2FYaRfdS28i3mF84zMf6nMY0dVV9SClncskBYudi2EpmqcTDK3Fnop3LVgcrS7Lev2v%2FRmbLPpTHc2Pze5x%2FMn9zNXyYzgLBnBBugXSbIOA%2BcParIWVB399EX5NU2gpXw827YOSlzuW%2FYcMil3ZZ6LIm6wg8dk59Bzj86nWU3&X-Amz-Signature=ec47de3e9355fd57a08a259ccba7e22c40ce40d2cab90d5c6284720653fa6989&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

