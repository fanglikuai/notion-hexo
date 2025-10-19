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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SIM76YP%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQCLrOlUH%2BPr3Kx9YhcVA2FzNnSEZHtoDDZY%2BM1OnbohMwIgV3CnSA3Van9lGipnHLHIaeg8dDt5Ddbbe6MUYMgaTTsqiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBHpzl2wKesvyycHVSrcA7fmb1XNcWIKANbluk%2B%2F7dZKfxsmqnn85XdAGv8IOsjurm4TFSf7qYoUqJPSnHYOEnKtgE%2B24GFzfGibYRW686JQ5vYslzf9ZBzH%2Fjke8%2Fqdx1EwHMCMYZRA%2F5XscgxsfndFHSjo%2FS1VQlGplgie2mMhkwj0aXZP0Yl9FBeV%2FrpcGZA0oRBfBlmWNNhr4AWaKDtdjCZwMUl1IYn03jZYJbWXQpxq0jqY%2BVfwMbwoIlbGXYrOkMr8r4sDZuHQ8eF9M0O0pb8p2LuTXzpGJ3NilooRP5GvJ%2F8nYWluv4r8B9MUzdsqgiHdugDGeh2JGhIWu3UbcuME6xqjoxzAy8bAc7n%2Fi7pkks7Ugn4eRQxe1lznb%2FwLUaGUnNXrC57%2BIhZSHWT3rHJ8HN8%2FuGrIvk2LFWDCs8cT9%2BkTk%2Ba%2BBykdB9%2B10G4tZ6zHj02t%2FSi5XKcRHz1Vd04z0mC1%2FTI9CIHkapHS%2B954IvMeLV%2BUJ7wUDGmCYzs5q7hXVUWZQAFrtYSs1GhD3TiiG964Ef9Igx02xttAKtAEZ6cBoGDMrobWkMWjUP7CXYreuFmuufw0cJnLkbXdort9GVnKTncpoCPrdwjv3428qHIuPyDswxe4qXu0j5YEgZRCOUAHQ0DQMM6X0scGOqUBhafGMwLVF1xvKwMsITa4E9qmCEpiG%2By8vZkoE8coBzSot8i%2F7ktkAiBUuLgTkXRxuTQqpx2455iEWOhFHJJB8p1tR5%2FocGcid669Pdj6TPV7wezV%2Fvfa%2FRQeD1zh8UymhLUGZPHaESA65EEvfeWRKQume%2F7mF6l9OtV2rTDVYXXieNSTnLnityxJB45CQIy1ZgYvhxoUsxNYqbHOk0KYPWFj2h%2Fb&X-Amz-Signature=0130a6409c2a250b733e6be51af67a565bba7b21a7c00a12f003eb494f62c7a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

