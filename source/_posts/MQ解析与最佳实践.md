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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667XDKPHLV%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCNBwofgP%2BxnSYF6EDd%2BXik%2Fhyj0IbpL7w8f6tHpdghyQIgANDD08IxOrcvwRkoH89njJn6kwQl2WN%2FQyegYLaHsUsqiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK9csSdt3AbJIgBKKyrcA%2F30MDKl5Ej3z80Q6MjzxwhCr4lZEz0%2BHmnoJ3cDF9LDt8ZlFe0xt9mEtB6zL00h7Sp3xm2%2ByZjk7SNnNpuwZkfDekthBhiNN2l22M6sv4ewZwEm9TuHf9x%2BHKzbOVndXxeJbgzzson14jr%2FG2x5l8%2BXgVE838M9CPH%2Ba4e9KvoS%2Bm3ryRh3pKm3EpCiTYRQV5e3iEqZ1XxnXZTUgmysWHpWKCTDqAv2mO5w4wuhbfEjKPyiXJrKH7aHnvvKWXUuaW0m1%2F0iozeC%2FF4fPtPm9YO3ITJIzLepjTHBVnpmeKuShH%2FxiLgimDeO%2F8koUyjQK4KDrYB%2BH2NkLK%2BpSQeYki7jkhAuAiSg5COQ7kmeazmSlob5ok1rs9DmSxvaOOzFMEKTUyy4ae9VCrnVI7oQ62HLAwL5sO%2BIFJKtAkxir9TjQ81xoQW2QACZDpN%2F8YZ9eq5bfiljJqxpFKNqqn50k0hSfnmUOfmRB2HSxHyXE86nDcg7TkssGl3N9Zq8FiusdHXNe3kOTCkXdsXJNK6rCR7KXvGdyUO6JEFQqqTmkyPxvnnH0IFRS8tB8XWmz%2FyLBNRoDJ9xO7YQIbiaFjFrj%2Fdc2tDGUt%2FI72oGOG3WueqLP0BzHKMiaD8BkKnQMN7nx8cGOqUBCravNtxo%2FqLdFkcBA9nhTpstRWVEu2d98yRwlIf6HBM9%2BTtn9i7ApB6QkuFdHyP%2B2RMGFgwwFqpVWvfW3e%2BRPCkVjWEBi0IGMs0tyAXO%2BzggIQ%2FB1oO1lvN%2FixInHvhwvimYbbvpHJOJJf5CZdbg9CpALjUnLDjDvcamLP6sQ%2BwyoTx3p%2F0DFoS99zn9udNTliX0ZwPp2i47FQyx2WSW6qsX%2Fzir&X-Amz-Signature=7012fd421c9fa3178d76a1b7447e728887135b47cf17709a4d5baf5952a1013b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

