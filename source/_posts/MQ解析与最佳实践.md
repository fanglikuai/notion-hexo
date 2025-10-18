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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EA6WK4M%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T170051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJIMEYCIQDI9akIpdC%2B7%2FUmqehFQmBpVFMWJPjjPHbDsvtHxxMcwQIhANoGEBtgBH%2FtHl3qIb3S%2B60wGc7OX20Daf1Etdg6e8HWKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxJBFswWVvi0qbqNeEq3AOZthziLuN%2FRw1m06MYb8dmHcWkJL8c5PjRFujXWMAt82b4Bc9fCDVXvpAiaJfXgw3EBDNa8WwoU2zgLCMdUlY97ko%2FYm6%2BPPG1j19QTmFY%2BNzOZsXkKwLFEw5zNNyNkVtHIqQ49Z6lGY9s6xgFTB%2BDRpSiJg6%2BUa8x1mbKqkVTdcS8xhsnneZ7VFW7Sx%2Btrh%2F4dRHMgTXkEyYSaii7u7sQveUSbr1AWbaOwz%2Fvli80akSSPOwXLIZftsWfSZPCkTtZXVgZYYv86Zh01hkZuhbhPamMw5BkWOMLqUSdpbZGlqj63sEyUkuS4v7lx23ToaqoznpeQh%2BFJbBXs8WjYc%2FRC3ouP8Yk2MjlsPASakc1I3paGrM7%2FSdI3RHvhxSBhfPSu9t%2Fyv8bvuSjt77dNgOIuxQNWTFRGkj3SEdf1dQz3NDWNnv2GXcrhRuwRyL2wFSMaELRgAcsoz2DK%2BfXGlXZ3gfod59KheUAvXVs721lLfJ2d93Esp3dQkzWlqEYNqOlbBAnCjnzFE7kcEwlY5iun8vittTCQ2wrEtvYTMxP9plyjupVWDFBdf%2BTL%2FyIzZ0b8YaERYu5NSVoomCq4zc3MWnZx5SQYmkuwPFA2PAFnuzh49Hu%2FLEzAp2hRjCEg87HBjqkAZzEdsjza6gXvryLAA%2BnyuhwDievDPfmI8Ik9sdRRbTaQvEF3hxR4SePmXjPsMBk0%2FDLnQ56TYtSQW0D%2BOAnlGzgQHI0mPGAA%2BczI9Oo4jidtu97WjuHX73WN42sw5WH5%2BZaCIURj6jqt1h%2FLI3CISyVHvlELupKzvsbRCX9xbHYBDg5Uq5qroUjTk3ailORB5Gcxmzfd54%2FWZtrRnbOsPEnSXJJ&X-Amz-Signature=22d0a55cf41c2c4235456cd56f0d1089b453b070f98e1ea44cb89c67c00b90eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

