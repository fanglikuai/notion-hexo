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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNPQCPLK%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T140117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJHMEUCIFfBeOt4gU8wL3TEY7MtyF3Z1FMwrwT4zqykjXEpd7D2AiEAjI0YwN0uMb9awZs3ZhTVjBGGtVTTvPisecyB3albpFsq%2FwMIDxAAGgw2Mzc0MjMxODM4MDUiDMPpE9Lo6Y5NywoUXircA6kXaTCFd9E0EX47Uz7dn6va%2F7A%2B5PzhMxrkfN34Q91ZPAgHeB9cMJA6XaqWm7u5qO48oFQttiIAnGsA3FI%2BpSRvq7Pl%2F3aBhCBVM3SK87SjE9DuOh5HLKB94eKvEnJYh4BYuGHUjMxYG1aeebbgNXqKYHKQLmAj1YFqua%2BSI3dxRPoRrQw1%2Bn6zigd801I%2B4B6YLDN2xk4ty19xp9tXWya5K2%2Fj7LhTRbHZAWqpgayBSF4IWGrm39tPLk5rH%2B%2Bev5Y%2FTGuK6syaGNKEUqY9%2F33Rg1olNL9yfu%2FlfbD1yUp6NmTycLbkJ%2FwGSSrGuGyyOrdotXPhWn%2BUg3FoRILn66AM3Zq9XgRs%2BOS3pCH6VkWhbqHGCodcrWAdWPChAAX%2BMuzvSmCY5UyBAMikWYrRYwQBcNYdnERJFkiIIcB3rUZROqGUa2ZJGL23KsPb5G7WL1K%2BhzIcGTC6HNqiwKGaoEMnavxW9%2FHcbLKcK40VnqXE7%2BNGZTZYUE2toGPeXEFCbj7u2hSkmOefV0SzRarYjCaFZiLHuexvYaK0606jDML3SbGOHQhzY1xFQM%2FqyAz3Z3JsRfpiPBdzVrP36jtIDRjXkCov3q5dg2pSlkCq66mNDQvwb%2FsE0Tzgb078MM3QgckGOqUBWG2KtFI2LepEgnVl2hIv3oZgJwWBmzrOO3%2Fl1axqk9UApwVamrFCnWXnXktrBCPb04b3IhAUl8kkyYvR5rE%2BC1PambVqXxcn%2BNF5VyMXkasYwgSyV0Gf%2B3FnqQYToVLVDw9Q2fJJ9olJ9Nw1e8hYIBCL9AboYSv7KnLca%2FE3SRYJLbFoiRN05FY%2FeZgXe6Z%2BaIQWiU6LtGIgFINXWiA6bbpCU3oG&X-Amz-Signature=ecfd5af1b73fe674f6dd9170c9e3c78285635278e83b3a5ab94fa292704b84d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

