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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DPAKRQA%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T160039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCcB9cd0EKnjaupmEOIxJA4uveMLjs0uZtHs6p7SYWhTwIhAJiUp5UHhxSn4yyGxRMy9%2FUwS4upnrlnNwBwy5Lv9XiMKogECPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwH%2FRzla1Ixfle0T7Mq3AOjhXRNLq9fLq5M9XjsyV6b8JdYDitDSxva1pZov%2FQgCgZD%2Bz41%2BIaTOxgAtEELNbfJyFaspSMRU0%2Bjp7wm37OPNE1AVYtmwZix927TL%2B2KDgWRDoxrj4OsmDmmGh8TxPuiEk8vxI7rs91ZDa6Jeur8HokL4etN%2F5YjQ8RRqO0xcEqqG5z4K2NZGhFSdx8tGetf6J5cLtMNKRyp4ZJER0LhufKU%2Bqmyj5%2BBbfRh2UpAV9R5Yd800Ud1xXndRtUy4X6dmURjMTXPCmBYFC7aTwHLU4e64cjC9QtQ%2BsezJvZHRDL08IX7lxLfb8togkLr4wEJ1VuB4%2F6rqMTQ6LDfWYkbAO7lch%2BEcpzTyWTbfd%2B97kSXZARKH8QccEnvPo65DolqFfnzsWBiegdveJZ5XIAGGC4Lq4vrhaki3Z3NXQ%2FoXP%2BqTxnQXRRfeFlmg1QEKtxSd5XfBCWn9%2BVIkvSwtlVxFA%2BISDc0EbtDap6xeUcx4xxvckpSyy3rX92eZWYsaI%2BZ4DQoUVMyuHUENrEZ72ZlaeGMM7QlZmZio4MT%2BntW0QlOjHQI%2ForfqEcHqMPgQ%2FnXd7otYn7LoVihLROAHe0AdM9YqjHEIRwsORhAKPUoX1DUOWHi8pJkU4O4cjCxt9nHBjqkAV7U%2BXeb7a1nhL9ZJ%2B6HQlEeJe57m64T95A07UM6Udt56GbYe%2BDwZ9SB%2BzZR6SOAinPH%2BAccr5I2K5pfcXYS0G8I7XX39vO6ySpPyj0R8SFgufXFEUGr0qKfrx8cFH4FnnsvRejIyYq3DIOMRehGAFkppgs9uFRLQv0t79KIHOlmqz69OL3JdLZ22na4tnfGph4x%2Bi4bTicPmmI%2Fozk7kCOLy%2BaY&X-Amz-Signature=dbb6e95f81230dfeed1a785109e413ba298c50b801880ee1c2a1abbd75739f27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

