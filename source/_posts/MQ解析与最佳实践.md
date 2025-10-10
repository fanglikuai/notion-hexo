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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMCMLX6C%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQDQS3anK24sX2RS%2FmDvfjYmxNkcupoI2tUozhR519iLAQIhAMHmE111hc62R1gxe8YsnxyGJMDFnRQd9Esd8hbjBsVqKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRE9R82%2BMHFvsJKaYq3ANt5D7VTloueVM%2FXA72aA0nnVjBCDtXBEvjrkjw881GKFAcYhiAeGlOZLD6Wboo3QNqOCqiIXgW3iFdAGB88Etc2a3MjWV%2FYheMhQMhGGVqA%2B7a1Va1sWNtqEDlSe8IHe7IRaqsnaWYt0ZZ%2FZWOXlPmvMimkdK7WdI2YV2LtZfgErHBCGdCkc8JwaIcdxcH2jkwXbR9%2BoP7WKgRUzEZM0Mpwu2lXu%2BIXGkdZ1RHy1A%2FsuSLyYEVMd73Tj67hOP%2BoTh%2BhmVGqr2jHbVjK%2BBae%2BWogcLUWF1JK8LJAKJVuYg4YuhOT53eGYUbwFFnfdsYmp%2B4p%2BLnGqR9knRFusggXEcnOzza0O3D7%2BMept9%2FEyWjg0XJJDGJWwewxg2%2BoEHiJuf5iWceLq%2BAYGAbW099D%2BLME9Ok2PqgpjBqAyHiq4%2BiTwpgJan1gPbJN5HrXvZtLm1mhMq3UWa3FUnNYlXEHpYWSY2iXDJvRRD8shI25Aulz%2FXa0zRa%2FNasAWp1gAaOuso18VHm3yAU8l52QHt0iGK6%2FT18latVpfrf%2FCsZUS%2FIVezLyIIG65mEDh0NH8Wm%2B3NqNQFkdbv7TZdq1nwNAuu0%2FwrUnSGscAeQVxLPpdC1kFiM%2Bon6DUH5MpUAoDCPsqHHBjqkAUWkl4J0KNc2BzXzSIyqDhgHdVOOKdvwNpGuWvz4f7xZaoYO0llkftwiS4kRSzMRZ3LngBNaRWm4knzi8BeOIgvNwOLNwU12R4ue5Wib7FMd%2FjPsz20SKQfJ32hae%2FIIGVZmMxOzrE0btYw%2B%2Frj4aJzB5GpKVmLZSo%2F4h24LXyGZzlgpzLqeoLQD%2FfHoc68x2tFXVHlx0M7fbcYWOKOhGc7Vo8Qi&X-Amz-Signature=d6745715e7e0f4583929de0d2bb19fe67e8c54e68d9548c17fc17293301325fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

