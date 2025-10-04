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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZSIUNAOG%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFZxLOcBegfPJMpgeof72Fkas52stQgMoY7znJ0GKbxIAiEAoE2qrUap9%2Bm4MxaX7vfatuC%2FQwW%2BFk5uVyfphMHh3b0q%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDOWiCngWLk9zlCu21SrcAzZIb7ZdylPQune4R3qPyBztXupndzxQoa%2FIj7HQSM3fZwzqm4HbwnjFbXPOK1rtE%2FDhGF7PTQ04V4m%2BNlXX93xU9ygyb9ZDOmhBZBycD5TCwd%2BTkY0USVr3rhNp6pgHQfrxuvNh6yVZGgLzo%2FPcr5fvoHqIhRJ2vq0oEZkIzUxj1vgWCVqetmGXE8bWftSyv3IeNu%2B7D85wdHuDp2jrns47KrmDmLJOJHKv9gfaX%2Bg%2FncXr%2B87CVXV1m7QhXx3RYF5XDJHzegRnANXpu8KjsGgWL6gmhk517Z78aCmZ1GK2pmsE8erHFZkWUyMiXMMo%2BVunYx%2Fu%2F0q%2BifLXAEOtdKZKH4IRRHdBa17O9L3Z%2BjBktCVi6Iq0WH%2FIA%2FFnITaMY%2BG0QJOIORmc1272QWHjnYDuxWicPiqKZmEM9Bl1XSeJsXVPkFCB1F7jjCaIBmTLEZyC%2FujdtGuqkTYjm%2BhsJtc7gADk1M73x1DPDub95pXKQpD812esfKDTh5seCR1BgxaSuUWMayPAVmg4mkDcksj94k6ruSYPQSbs%2F1SqFemuWaiHxkT5quHWoTKSFqwo0RA6eDYLw%2Fg5kaPgVDLpbB%2F028JzHdcRHo2aevtMll0jAxaUW6zvDgGFy3uXMNKbg8cGOqUB2dR9WgpWsYaFeMCDNXtJLrb25DLyz7tzJd8Gs5n4MZVIgri7bI44Vhgce9syhjnvpNMvgKKsqu9gkMAk6Q2tAue0qJ6tyZzAVZu95Zy5u8yPCdY%2F7BHf25mu14HL6liZTyO7fuHZhQYf6GWw2UBoKf%2FH45C4T097jXwur4BHvLakMfnPQ5J1MrKcHQRdhcYsb8Mzp2Qi0MMagUI3Iwycce2k4BKG&X-Amz-Signature=4fa001ec70e07bcb31c1322bcdaf00523a241b68a4254e5c939a5382e0c7f197&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

