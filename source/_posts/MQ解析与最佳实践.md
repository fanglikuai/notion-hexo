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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JKODN25%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T100043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBn%2FYEqPVTNScM7nL1Oty0BfmdxQxEzLQd47MTrsq2HzAiEA44GcTeP79QmkOBBHdFrxVL4yZfmXssNXvKJ1Ni51W74q%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDGUJlM13MfwfoDzU1SrcA3rVo1CedfQlUh%2BKgGH9VkFXx32xWt7T0p72BB1k5wUgiwHuVsLmYgl3PPYgmyy77vH6%2F7O6IrNGbz8KAmAk03tGWTxxFnCAFY7oe%2FIeEYK3LDEuzTHb425FauDKIzqvHlDS2iv4n8e9y2w7C35XumMa8%2FnjKEsw5Dxf%2BQi5GwSKR6s9KOJkOqvlxHErf30HXPJzRvGtyU1xjNtC3LXvx%2BAmChyjoOofar5DaYFnrTad1k7HcMt3qDThIasMy803G%2FM%2FnYOl8G6cyp%2BTObQsD32wk30sNteT4Iajabr2cBHqgQqnLz%2BtlLWBYWj21EyNX6hDCAtT94nvuh3ikDliam2%2BD%2FAFJUzGtqk%2FO5ghLUG4fkc%2FNh%2B5m49QdTY99rmtDQrHeqMQe%2BwSLYgp0%2F5ojQwPfTVfXpwmDhnqm8qaAZDRR8j7m%2FO9i2neJEzrLLC9iHq3yivWX4mYM%2FvvZDnwKdaNvqyVRUIUh1haUy0nTWrVb8HsrRI1uZWO9XSJU0tDgezm5q%2FGseON%2BlWSMa2zg3w3H4oG3tfK4wMjf8XSDGOYV30v39aLh2hJMCOSxQCnKVHu36fzZyeIn7Hr9mHd3NQTxWB3twR9%2Fwcb2nU%2Faj5eJgA07VMcTMAP6SehMLb5zsYGOqUBMeK%2B5w3O%2BEdhM5ZI1IlWTStYlSxx0npxZ3dQepGZpggicS2K3H7hea8Myi4xFGxxIfMr3QoaVKtzKWUy3YriK0kDEZXeganBLpQzj3Nv25E9o2KxlhlMdptBKSa1OCtwy8vNyh9zHIRyqAX1h3a3Q%2F%2BxSZ9FB268hjfkI%2Fn6TXLVRKGuki8MUuYbVBPyVlKpyJSrdYVp%2B992Ma5pqvKDRvbJpfh7&X-Amz-Signature=3c7ad20bffecb30ec8c6cc6228607f0fc1f2bb6b8f0c731c75be025336447b9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

