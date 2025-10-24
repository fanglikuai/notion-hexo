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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STX5JA5P%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T020054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDQC3tuZ0%2FuO5DnkD%2FSV3LoLJX8oGtmnlJf74ot0ROM9gIhAJuwcXfaNQYMVVV6jPOFjBzJqtldYDiNmjoB7z9ms%2BJ4Kv8DCFIQABoMNjM3NDIzMTgzODA1Igxdww8WbheFBhBLZC0q3APwMDKd2vRWxmvlYcMBXemgJRUwctPcTrFl%2BgnxrhTRD2QAhOsqU9XzutIgJkI71tsbDKlEZ%2BjiQV2uwdigXGRZ54fUPJjURHK50ylMCIjc9lLkl7Oh7W4WqevHHJMDEiw0Itu4oZmclfcQOWRmsqIdvGeuJlRRseVAWe30WervZgMILQkimZ0GnJ1FwgLCz44jAPREfjc8QqYfH1L4r9eBquQ8VYubo6TGd7mKx5QEnPN0rbx%2BHLSN%2Bq88dHQ6paRXPqYOHhXWQodyL5FO22c3FyZG6b48qhLgd0OMw0Rzu89P4WiWshVi5GYFSae1683LZgtVGz3JgVCzh5FnlszVw%2B7GnPts8Q2c50U4%2Bf%2FMHHJj19XX9q9ffbkeU0z89MEAYpCMmk0NG391bhJCk9YG42LyRJyGHfJB7Ed4vaLnVhlQc%2BoaSWbjje9Rqcrc7zPwnfnnhbKKkNX99ej1I45LdeWRDyqKVfZAx3NGSWW6W4gy%2BcjjcBCjE7R%2BO0pC5qpo0YVdRsd3uWXXBRum%2FrEBm6%2BKo2%2F5H68RYAg5thbDTurS7n2A3Zv0y0db4w2ahnYBesTBdU0yUs1p4ZYX4baiCTNEK54%2BVSz%2BXB47jl%2F6z0cd6jRlgqzP9ThsZzDhqevHBjqkAcUXNZg3mD5lgasy94xeFbIMP8P22OLHw2A4oM1dpdiITbJdILMZWg30uuQr57eDTg3rjM3wAednv0xvJodwaLiNuPsfYuGwiRJ5WhS9kg1kwUENKm4kg4OqzOwSUyEAAOmAQS%2B3mSe%2FqOOCkbBclSLYhcTI5d9fLW%2Bp9pXkJ6XmwR7fveX%2B2WqLOqm11hWqrVETYc%2B29nWIY%2F%2FsTNirIU%2BXdBuC&X-Amz-Signature=a82871f60c0d9cce33f02ea1a3b71604e2ddfc9d6bb644a250fcc8bdf16e1aa7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

