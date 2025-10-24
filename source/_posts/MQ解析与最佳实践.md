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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YEZEXL2H%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T160042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEVx%2Bjf1%2BF4U%2FUs9BnH9MeVzoD67mS8iy30gGHv2i%2Bv4AiEA%2FYa8lxPSC2QDRg%2FX0a7eBflQBksh4LencqomoghfqSIq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDLRNxxdefUAtN0AjFSrcAy1m9FwZXzqxj66vJ%2FPEzJesvNv3GdR6l1XjrOPZK4%2BRH%2FYJeQeAaMchBeUAt64AEzQoGvHfjNXV7y6JaswjhRQRWONd%2FAvOdqh4rbN4VbDE5N8YhO42O6BdtlcvY0yBk3bmitKVtTpO20Jo%2FItjWDpFjnlYsnR9PTVAxEH3v5cty0K6AX6kE2Ecu5st%2FIGDsqLmpP6lhjpG8qiQmfRpJOOlSgUBy6deFSx%2F7f5Trh8G5WPg3t%2FNfUMYsTsVTkq2%2FGPfpseHBbhBqNuY8ROg0YWo0nG93S2TcX1AtrNHaEprzYio%2B2JGSsHCYxrXhu5n9FnCy%2B6fL7qSdPGuRKwawi99zCRpquHqfhTsCbgtavEXld8iMFAjgbZ2DSFLIE8OdgF9FvuwPbCUjtUKukBuYpHJd%2FfwIWfUvvUMlZtgd4iXJ0y0DGcOjVw6w3981bplo6yVZLZRFtZnwIUAqxgJMeTYhoU22QAf2iqCsSDuNnz8q%2B3eviBer9AS%2BfvhsAlRRJvb8WYak3xSKaTJg9%2BBQhplSbf6qBYJUmEskcrBdQMwq7KjDBlYGejzEYWMQMNTiTqwEfhJfm7b0pjYPmlFLJ5mcA2tG%2F7DUbaa3vEGkWtcxDNXNIdmf0nvxvYnMMqu7scGOqUBoBq%2FDX8MdL70s8g3RkVxcOsw5Z3PR4xw1hnhOTzzJrEVsHWX8uQgJWHqjlomfu7tWdrfFNG11f1be65h8QBW9DZmSnmW6AbkdU9l5n34r8LLQxL9qvrdy8VeNNLR3kOSUc%2BinlA7o09CpcnOXnxbe4vcc6UaMzE1ErxhRBBwm4vWZ24k0dsWMtbLAWqYJH%2BDP3ReaD6Aa0fVtrUGki3XzxVuHzaH&X-Amz-Signature=5d4f8027e1074fea874d4336f5cbeac64aee67e47e1c821980ed39c273f2cc1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

