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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652XGF66L%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIQDnOBBRp2lX2Pcy118ZJsMJtHXrojDbQca19dxJ9%2BEmzQIga0UQtzFVzeMw2IAXkZHwIUgrCx1M6djsy77R69pEE1kq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDAQg%2F%2Be6z6gH3kF3HyrcAzgw88GkGS9RmlPRMaWg6MRZ%2FLAEJyQQoLkJwwnDJaos%2FjC%2BUieVFtuPGKb2Wug7TYsiVlcq581Eb6OyGhQnOxR1PZqdOd%2FkLGqwgz13m442Z%2BIHWaYJ6u6B59RKQuZoNTX3NYHOvZcbhcSCegaWoe1U05L9O8FBRISuw1O3gfcZinIFaydJ544%2BUmUXXZ1Czp1WYhKBx1SNfmvnkUWDdisc%2B6ujrXPSMGLDJwkwB75JC6mhyvkqf7Y66FVKTePQ%2FTa7Ed3WAWq6zFOsH47ILoLewhBAWwviXrBaH3%2BY%2BFSFJzGAj0Jmm1NTFNV5s7nObq3aPtpxJtXcGQuWM1e3aJGNccpFB%2FSDUh%2FR%2FPeEaaMhWI%2BkVhU1schZkzib5OZAwhpMpGzD9p3TuoXp%2Fe2Io5kTcSvVRR9U7K2nn9%2BCkLEEBgWSdvX09WzqqM%2FvJa%2F%2FF3e3kl%2Bv%2Fhaeuy3wXa%2BJ9kfGOKiNOYNfesHl0%2B%2F3oN%2FONYA3aQSxW2RIYTZLnkf5V3k8Z6hwlx4XJb%2FtC1Yd4w9JVo7mMqxt2vGep20SrdjhEECH7rd9uGemURggikA%2FL28TXv5zdOJMwFTyP4cg9bWzDecIi7NnB5XXswp1KuZRsSGTOqosuf2qhjOeMPLnycgGOqUBzqj2o%2FDqzZEBUH6HIZDwLE7YZti6huYLzxVRapXGfKYE01wtfOgSTguPqklVsAhsv12vPJblVE5sm%2BRza8fFM29FSHQzEgCNwEQnmpSFiqCoXVEZmkpnkQ7wSMxO9vUWB9L4h4peaNXX2VH8mT5eZffFhHn2irwBiJiw9nen%2FnhlZdGSvBIV16MWFept0u1UdWoT3VFci0eS7iekd6Mp9XU40z3C&X-Amz-Signature=5bf3312115d57fd46283d93049a55a015cabd110d89ddb5cb8a61429e56d70f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

