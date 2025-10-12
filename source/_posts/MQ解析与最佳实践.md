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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCA5TEWS%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIQCLtTghYexVFTC0GKT2twnI5wnrJnR%2Bh82f7ol9r9XWPAIgZwrQCPJh1y%2FyUUSsX6Gi96pebCj0zO52al%2Bmgoh5zMoq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDD4SmOmzoXdPUmaurircA4paVpLbjEN9iMXO12kKYaxdGDOOKZWgtPpu5OjiUlEeVS%2B3Gkssut4FtPVXlKQ%2FGEua7RPLbWu3zZ5%2FKGe66Cnj3jH3I7d7T8ofAOmvqhFesNHrAPDbD7WR2tir5%2BBwFS%2F1y0ZEC%2BLWParSBrzyEJl4aXECzpRFNVzIW%2Fy0X4OV3PRwf5EmOjvShuGUDezu1UQ3Pzsp5N01jnb78NvlQ3MQOx2gzbmrXCB4Cw0m2jzro%2FNNq0r3rWzhRgU%2Bosg9OgvxJmmDP84pwl%2BpKr0hNoftqjPVeN%2BUhm%2BzSWOhJhTXc4jn3Xbws%2BTvxEnZ51lXQDkI4Vl2MVA0%2BlqNXJLxTZ%2BlPkudLqOV0pgcPu89xHxN%2FqRhu6j3nNRHybp2NQHaOtistLNpAAzCk0zSE66qAdPcIx76iXp9TRcndvAELsCs%2BqZHglqr8DThpGBKhj0sSnnWg7qOg0ERIrBR3JV8j9obo0FsEyvW4qOEVDJqzgiRWBfJWvB8SMHjgvl9I%2B1PvxtM%2B1FABfVVXVe4cLxZWBQR6bC39C%2BYLk9GdhtSmcU175yU9usgobYjlBSL6gP2BioShA9IceZeTr6cwhpLnCDqo6WAITmj64l0A7GoVEbDZ6Zyxjr0sKuWOHHdMNTDrMcGOqUBeoHJ0HaXoirPvIFar7XuZkWrqu10L7DiVUGA6JfmIi6mxYa4IJXyxb1BU31cpzzA3CkpNr%2F7Z4gCSuOanN4KAtzwTKmeHTvMgTGejBE16%2BrfbTN%2FPzB0%2F%2FgMwC%2FzS6HAXVq4VgnYyjR4rlsHjwX1%2Fu%2B6KQQvqmUPP3vIU6vLvwj9ISQvVdBe64B%2ByDT13rCui3H6eBvAl773R%2FWy%2FIQwAAFZkybQ&X-Amz-Signature=49a101e81c7cc5827903101968dfedf8aa440b8e0b0474b0466fa34f7ba31d04&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

