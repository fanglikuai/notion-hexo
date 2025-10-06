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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOV26MXE%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA0j5wp8aHoIjO54INl492w2EPuCkWu%2F6baM4iXreUH3AiBcxII1LFrxgOTo75%2FTwWKy35n9P%2B%2Fhi%2Blph9VfD5thMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMNQfPYMfv%2Bj%2Bq0TKoKtwDAw7xGOK11FLixiZKqhuLPyC7eI7M1bs3I3qO3LQBQFnY1HjDGcFHFwQ5igKaNpVY%2FhHos0HhfyO5VH9FUa0ByWL9ZDSxdnSh3o%2FMqng1NoEvptVa0qgpZXF2dvjzVp%2Fu%2FHMRRNcyFUHr0gHdHFneWF6Hl0vc6ktXlte0wQTIxyQBAWYy2kCFqPt%2BUB%2FbZ%2FXMw36LDG%2BTKWmKJivBb8ta%2FtzNZyu93KYQe1stOMRMBygt2CAvlKNk501m46CRCyLPg9CxqjmqU3t5%2FhdnOAHoCMBThX1%2FOHwkobQ9zSdeE76LojutGJYoh83lqZpOS1LWVuNyUpwUrVZS%2B7qjO4toQf2A6dPi5gY%2FWccGpAvIWzt%2Fy4%2BlfM6HDuWivlxji1ex3gwT4Hp4t0jZlYerNr8Yedv3gzKqZx53KIDT0oGmAR3s3I9AX2UaPDwyPvnqbZv%2FPseeZ%2FgzAvXLOIACIBP3wuJQJCk1JNAgZqlgH3LlKUwjpynMALSlaDOZOK3qkif%2Fb0kN70B2Cjm2p8bfkz1fi6RxOH49sjWqbV%2B%2Fx%2BxX%2BTjquKbNIGCFnjLgl%2B%2FGA9zZ70jwu%2BSVSdu74IPo5hZNKtqHjGXv2XzSgSMtKWebvH%2FnWXjTrC%2FKoV8lS9Mw99CPxwY6pgGOIoLkNkOPXBkqc42WsrtaWsMEwILsrm70SVsmYW2KPYm7fsX9iFFlYkW3CErdVpZNJ0uSlIWfITeO2%2BjPMQNUErScAQaxeH9vNWzXYyBB8mclGJA3nU3rCUHaXym9lI3HigDr0WcU%2FfOU7SiEvxl%2Fl%2FdVRAodtxpMBWg308AWpwGZJF1n6AeVXVV4Vf68r%2BQkmtIw1phPrKaDNP44X798laQ2ymby&X-Amz-Signature=96667f15f347baf1c8bdd0c895b96c6ee10d97693e6f9106a0e91195d9014c6d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

