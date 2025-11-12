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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BNDQB3Y%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQCEB6TTLphLVgwdoUkx5z%2FkZ5ChRGZ7%2B11bI3n7rhmLwwIhAK8MX%2FpDW1blkCwWRjb0IkpnBkWhUPrgXDocqqZIutiZKv8DCCsQABoMNjM3NDIzMTgzODA1IgwQdt0zKRC%2BLLqNXGYq3AMHhLMHD3%2BDPsZc7OoNOH%2Fs02RrQdrm3LDenPOSslzhLrdn%2FqsZRpfbJlhHXSdxGe4fb31luMO9OedzzeBdnUTr%2F%2F7%2FXFN5lLMC1IGhxmhXQUcmwjNvoCN32JZy9b6G310k4nDvQKJFRPLFV7qQrVkTdamQJxIEV1%2FNS0ihburJYW75XhUlFH2boqG89YUNOFUSfUIyCN0k8U5SRYKs4L45DraBKyGntZH0mEIUATDT2hVxMS8ckgmFgMQxNeP14JM1vpTKSEU3Oexf5VDuOtjJRcV%2BHmYMBmfYj89oWIa1sHS8IiDQWj7LjE7vi6%2BBteVvjwHZoToSfpc%2BMP31WK1mk6bxMl%2FCunqGcoAHAhgvKBiDbt7yoKKwMFSxmgFKpS%2Faonam22Ho6W9TXebzKVzWelz3XeEy7EmI0UG6d7m1lSAWuTb1Hsm169VQA5SD7n4ldYbfsKGYuc8DACxCKSaXkyq238jsbxSQJJ6dq%2BM2Z%2FyTPUlUZ9zShfe0PFiH8Apr5Lv%2Bw4a6er5jqkNF%2B9Q4R9%2FsGk%2BmJkoreGh2TGExSTwnCIj%2BIIdWBtEyLkxkblctHUuJDQoYkrHcCFXV9kLLxAvBI4LWtrtjQhlZTn50d%2BkG2d8InXqoL%2BdPpjC6zM%2FIBjqkAXfDmq5eUNVV5%2BgCN7Gd6oV5vR1wr1MgXFXamixyC4X80BH8fReh2FonvjR5aYqi5ZkQUgwvs7wHzIZKZoab7p%2BNy0rzl3iq7AtfkBNgj8fqDltwAYTs1vkjYkIAK5ReJWPOFeH%2FLbk6kItfjxMvR5UpADRcLDDmXitJCu4eRoh9ltFs%2BGYjzyxinUBuM5ogRHb4YDFvAM%2Fo3yZGWXeDJAEdRWxT&X-Amz-Signature=f1ead176930077c0832d1b7ba66f86c1c9c8deaf5e20f15a247eb1c7bbb02ddb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

