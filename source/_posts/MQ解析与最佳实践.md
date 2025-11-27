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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YMRRZYO5%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBm72T31ZAFDXOkOni%2FcAe6jubnXWnhdEbRkE1XtrFslAiEA1kYXdJ8Mvdw64V135W3g3aJC2LhdEK2y1aDTbMBqhG0qiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDF%2FMaaYSTRsPGVOgircA6cj%2FLD0O2GifmxORFahwFEZBjTbnloT5YwVJY%2FjS%2FZ7O%2BtaGWXx702U9sYKDvGm%2F%2BMKm9ucMIRI4zY8%2FFgGeBq4N6P2PjP1u1BPuNhMk%2Fztz2F8fdWfgAyCxFZEK3pZanQ%2Fr79HuPxktYMoZ1fci2N%2BHccOrpEGGuFP6QQAAvA7MsHW%2BaeCBIfwWXnN0zGqiMWY1hzD3%2BwmhLwQNZ2p9i8dCPKE9Wf6JJLjcFBfQnxI6EN7%2FQjFK%2FWSNzOXYSqNHUMTqKzZ7eAZNBdQmH7d%2FRP2%2BWXywrlLhU4jRTeo%2FVuVdZB430GV7nS8Rwg7vr6tcmVHEUhIrpA53aW6J%2BBMo1CmMh9I8T%2BCN5AygTQiiJwwpDf5YgdrOafoyTI0LrRzEZG7dlgPr7mjaB2IWa77%2FFlgcKJSCciR2wUuuZyBM5WrL0pTPmoxOlBOCK%2F%2FqPPwwb%2BNeSzyWM1GgIOVepGi0gCL1RyByQP2e8nH3kGl7zTbxm28i0q7lUQoe5ZaCumigq5Jtly3ixAeoB2mNiKK24d2who2PGSU3dl0lJ%2BaWQtKHBO%2FPuEoYf9j%2B05FDMucNabm7TD5ww8QooG2O3wUrsJEv9SQQPZxsGXZ5S5r%2FHcOu3nmjA4IaSsezsc0MJu4nskGOqUBjniCZiku%2FJRJdzU6Jrxx5XGJCAPfJMhB61nT0PWBJN2qnqZ2pAG2Mszsh9eRgD1hxLBnAywDMfdPDkBwlho8aTomWsmWyZQZ%2BMLv8%2FSEcoPxBc1wzzUhGp3evN32xxtb7Y1kbwS8PEOG7DzWC8NS9%2BKf%2FxJbmq96C%2B0yZnosBsIwYZwwGtUKYA8XQVX64YqAt6jvQ5CdHKFaasBWoRdYd9v2sYy9&X-Amz-Signature=eb20a27ffa9ebc57da4fd4e256b1941c5bc3f7d1d30e7ae5cf3948469d0bb700&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

