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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WU7NXWA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbB1mImacR%2BYp0gkpViz0FZQBKy9K1G3oT9xSU9FDeEQIhAK7dbRbwvl6u1BLDmiJitIhK5PtOFAqAGl3JRNwzU5BRKv8DCE8QABoMNjM3NDIzMTgzODA1IgzRA0ynzAV%2Bwbf%2BX8cq3APlfB6ERQefA3gx5CUHXS10H3g2O913XESGhOfwAc%2F7%2F9YFaEyQ78k8fvtf6bdSM%2FG1A5XhbwBYN46dmd%2FTHc3ZNJ4Ahat%2BGNbU5doBmXoZ%2BjPJmj0OX6Ol59wI2DiOzJ3Z%2BB4Ljn5PB5ng5EZaseI%2FP8WWqdM%2BUKFOjiUzRGvXkcmfHPiHCgrEuW%2FafHhBpFoEh%2BOFLyaaaUuYIAwjOM6F5xeICV0B%2Fw%2BT4m4q%2FATQK0OgJF53fw9JOsdP28efqppqaw5EAJ25Dgvhg%2B%2BqGkJzi4%2BJJfCMcBaFpZzCHraIG3Kkr%2BS7fFGPqdWP1ZjT2KtjKhr3%2Bbcajw8WyG2vN3rcs3CiKoPL3XBEqlUFmJydU94lIlSO4mXJtKxfoqTKmEq6fo1nTst49TFIA%2FXG4MuugTZpk%2BS76UQdshRA%2B1COgiB37pQERJmdQgK%2BrZKxFM4q56WkdrqkAwB9tMpXxZ5%2BzJe3eaWi7nFBxqeOAqbFa%2FWZLJRSEx%2BLbyeu%2FYd7u3k57dF2Syg18vygHujrBhbvXu7vNQgx2hYEDIC2rAOcmFVsH5FH%2BTrpblm06ijzb3xyIaNDN6kZlOaS2kXKWVBFq5fWWnioMf74BIm6rxmIS7efA10WRDmXScSE7TCbyOrHBjqkAea8Mdu92zGJtIk1TNdEw0ykgcQvI8JC%2FtUs3TNOayAGzXatKlelxlyj5nSVOpGzdsBIaiLAcI%2Be7v3gwOaFuGGHjiAvpOtqFI3PijtP6w7Q98GDSmN7G2zYUeOZNUaW7IJILs4Xe4Sjj0tUMYwAklvTieM8tUhXngTvUJ4Fq96d4r1wvjEr7wTpHQee9PMBIKN2E0t%2FJGFXBVNlDz56G370DyNe&X-Amz-Signature=83e7ea803f8a536c8ec6be89e58e51ca2018859cd52c01d488989e6a25abc054&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

