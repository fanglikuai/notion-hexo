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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SIIDKU7Q%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIHC0gg%2B3R%2BXLd3VCskeS7DJRZ%2FXNt1z8fnRBVvpm52zuAiEA8sys6zj8B0YSI3qlj6RBVDp%2FfI6qiaqFLPB5FktOYZUq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDCzdGcZPqoVaXylWuSrcAy89AQ4%2FJWd8xc6cRp8UyrpsxBVmftKFzXzjNlbbxEhNRcXQxsaKVzByz6ofT8sVhbIUkVbY%2BXtVBbMHJ94KMhnlw4V1WiBrSv%2FUHJgZ4RW0ckE1FCzXCaADndxQItavTNftKWa20M7xFH0qFygSzbfa9HL3q9VIgPDgLvBuw9WhoiflsCVYwXGJFC6PY%2Bb04WK7lwReWDhJex5sTSrZFePm4%2BWJtpLJYtA0AjrHaRVgUoaIZGcLg2GxTVB%2Fy1CwRyGf7cozLf4MsKEuADY4ko57B6G6pIhiT55TZcOzcPgBmuI1n34GvfZPA5CZ76PgyW9ohQGgm4Zn9A8rvgeyY3Ml4qNi1f%2B2Hb7TUllWGKDTX4lX9x4y4CFZyW7VW8H3JZ3CLX%2B2s4%2BEZE5Dm2y7Qv4E5%2BSRTJbNZgtzbAl1G1fGuLcpVlEjDGZvisZO9xXOd53q4lEgX%2Bp2XTxzvk8yy5lPHIQ%2FDMcx17ZZ5jsyVNqIaE05OKXPCRw%2Bhfn%2B52GV7mxQtQ%2FL3BAN19kqgAsPTvtsDgx3v7S0%2Bt%2FRHUIdC6MDmUT0WlA%2BYaT1WjGd5lBDcbw3Ge73FM5fdTBqa91PxxLImrspKT3jB8kpLEqPhNBus1v3jA4gD6jW2NBDMJHxmsgGOqUBwYlIbF8sWF4blHIy05OPvq8PMAHtK3W28%2FaVQRsNmdR4CKJTTRXXQJ9hcdBN482sCDL0OIxTwvUoLzD4BJnO5CsbGIZgGlJyxsU7NWrjVbO200BWfn4FLPGzjmPp%2BogGTcWEkxKZ%2FXVxpCIsV%2FUy24ZqufYwNTVVYqEfx%2FPwmusvSQ2TF7nrzCeYERJofIHFzqylhmXz8Jcth6hgb%2B1XjtVNz%2FE%2F&X-Amz-Signature=b07f36e9a9cdbebef3a72d3f922911cf43427217c7636b7ce06a1f94a40f09dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

