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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662R2ABSHI%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T160132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcOvJzkkZSsW69HbyAWPiw3QWNvmSD8sTE9e%2F0nTX4JwIgajVS6gVyG3yzm%2F31%2BDT1x6ckKajs7JTzX5OHDbXlfvcq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDPKAIBsaauc18sO6zyrcA4DSWwLMuyDejIV6Ql4I3jrPbYa5cw6VKt4rsb7iVVPRmXx8i1E6Ag4sDvK5a5YyoLLnUHg%2BrVywX30hgiSLTIP6XBBv96Bs4a8sXbLNUbU4Ng8YtmVFX%2FKWuWKtlBFMnzD6XwVx6SfiuAwuY770MVN3pPKILY4XGe3Na8HwywpFO5RvUmGq6UJnCxnSISQSQexQmExfNvVf1T0%2BkrZsMyiGsgD7iv2Ye%2BBbAsOn2HU%2FjvXpEbR2TnFraLmUWr04WCKWc4L5VSbXp8kI5Tn1wbBODQpZ%2FYEWhO9gVlAvwSM14ieiVhPd9kLEEgpMlap%2B2%2BhuHPkylo2zaPSVSXEa%2BBiWTpugtzpGOI4DYmcKJPrzaT0uRT%2BUoxOnqHIxT1bLHVI1Fl4xEvDBvr9qtKwnPL4HPSSSReHwj9KKTzDVwHlKZM8z3TrGxwLYKb%2FiY%2FIZ0Qq4ru3LkF4%2B1FB5EXjb51CUWnFfZHuW5q1lnOp1%2F2uqu5V5lWT%2FnzTh9FXNogoMZJE0OnZZ4pWAonSAu6jGK%2BLOJdgTFt8yLSruB%2FpqF07GR8pyzkJSzeD9diOOLEz6T45TZE82ErsErA25wupa9MAlOC9QNJ0Yb3Fd%2F8uFCYIUH%2BdonnXfNuEiYF27MNnf18gGOqUB9LU6VE80cCUlQySdf9yYrxK0A6Yi26Njh7QInBxyNc8KyPIq0lcg1vq9rLDDOTxyUm0HpfBSG5x3c7kLEY6IKjK9FXrLrqOa4dzgTFpX7HoHh4aAwP7AG4kT8QQNsNaCYODueROt3VYcT%2BEqWucqsk0Ta6Dl7IQhNoYfWDcZEAYDAgmgvQTHWvAJL%2BHAaiHtzGDaFPcHcfuBpSdbmFRxF%2FUWWer0&X-Amz-Signature=54f85f0e50267d66ff6599c93601dd408d5ad425e7430db2fa1bfae0e6ce1007&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

