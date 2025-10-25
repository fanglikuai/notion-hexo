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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3D2N4EQ%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD9GZ3a5WVA2vwfCn6BdJWVlIppIynTgD7%2FaDZ9%2FZV5swIgFQ9VBhQU3YrjzUZHFkAa8462xieYbKOcErmb8a6f03sq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDBKIlD6VTU4ZDPYh2SrcA65WysDtEZzZMl8mNlUBL4lrhHxbLkhEetJUcM1ij70%2FfeqxoHDS2nrXVpGwGBtUOG9m60nWxfZsdP2R9LNHg%2BT3pYY3ZDNKqILvsCfWyi0u3zvW0QFtRmebK%2BVG%2BYH8EUz6A5xyKHJ20tr7Mex1lnVlQW0JBWbWtQDLzcstg5MuSdm81VZNf%2BtSxAYJ2AjUuSwASCtF4gq%2BFTetCtgZjIl7zoHRDnuXVToY%2B51GzlHAz5niXIsGKxDPsw0AzygohrF%2Fk9do7seStczEsb1W%2FZVPLYVTDyS1UQW5ffM9Vyow5szsOneH8yS3U0QP4YQMn9iFWQiLuXk85SNlDuPCupqiIfVSx%2Bdn1DbyNc7jLcNIT2ju%2FVr7%2Bwpe%2Bp58KAN0Byc0zjuDMvTTHPGGjQx0Aj6YqHvwkrc1IVmkr1qQ6z9Yqh%2F3K2nA6BBxklZ%2FdEOTgYC6zbFimBcc0qrlzN0Pok%2B8Kg9a7Y9jBb0fRZ5ukhLIf7tJT2Oju83hUTPwpnLuJ5SZkFr7QaH%2Fv1c49Pt801qj4oRUfOnfKzLodbVX7PO0ac%2Bzc7i1gqKETtJHpUD6%2BcbotHqKcbaHpx%2B9T%2FepzSLC7d0o3eizrX3FURNuIOqOrkdEIlLW9pQX4jvEMP%2F088cGOqUBKPV28kSpSvmV3SyOtZhDEnf9zpbYTvRTENkoLCjUsx5wNewxZpYKXqyk%2FE6hYc%2Bogw1VB97Ze8MfzS%2FLPu069qrRZEB2ePzj5KKKyHlaNCABOMfA5hz54EvIEEoUnD2UJgCVAQIyCa3vIt%2BkjfEWou8AJ2B%2FglTw%2F1zWpJcPHpoIZihGHa5vhRjQDPH%2FSJIA4tHlD%2FQB0gacnW62NocbGg3V1Eqa&X-Amz-Signature=75f1f1f199a1e749a2fe5175873a2f050f8d6226d72be34cb4ddd9c760bb0ddb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

