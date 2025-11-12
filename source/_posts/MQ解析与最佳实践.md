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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZJ2IYCSN%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCVKxs0r8hC0ZCukrErO8fVwd0mzGF%2FN3ar%2FnwYveJ7tQIgQlGcwN4X8Fi2jic%2BWcQ2eNW1fxhnlsDPfQtIM7Rdl0gq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDERpWExtJ0bYH8XxlircAyQdjq%2FZ8OXsSIYhyjy0S9K%2BZ69dIoUW%2BRvQt1Slw27Sl7RhOL6GxPXYLiETjX64ixHnVoiBr5aw%2Be25G%2BtGmXST7m6D%2BRGlgct2mxrrFOkINtI5lWdyniOdLfqVi60Rbupkbi1LVguz5EzeY9hBzNFdFv%2B1xZLY4R%2Fv53cGw5vbWP%2FNYGEhbX26JQX33%2BgTOWrx9CCTi39DYgVM9%2BjDqcpDgIdsKQQDRqDTxA5aD6nJcnEFaVUphyQAN7Hmcl6B255s93q37ETItrEPTkuXy1GvdrVli4sKi4xSw3QobBLqDJ3m%2BOLEuUjcyoftzd1sQzaLCrBaqGq2kB1j5bV233ozmkfod8UH31F3wiss9OnodtKSf2im767qsJz1ruayJm%2FjeL9Z0YVJlUKa5dTN6QC2uFE5IsZPaQ5Jl5eo%2F98prNM4fSmhTeSe%2FaotZr%2FT6JmErm88sDMo8i7oLVB58gS2qB5taUrCJR4ca4JiGBf6HIfx6TU3sYE15jkcf7mrMf9F0G67wDO5ChPSGWAeNMrlys9Cgft2rsw7kv51Odwv34RkumWVMfYhXzt%2BH6V1CeDE7BRhGGyJHVFipZVQ3vgGIO8n2%2Fro9jmPeYVbgFGiDa7p%2BC6bX9fwQJXQMJ700MgGOqUBfEDh8t6kTBcwZlYID%2FhvVFYA8Tgz2Qc3dFNcBw%2F7BAddl%2B3ReQ49eydsLOJbUoKE1kGks3IV%2FCR1JkG4peK3%2FdLydH4lie%2B4bSMLc3HD6jSHVwWyI1uiwscO%2FFon40ejiOYfnoeHUQCzuXPqKl7vQNGJFp7bgWMIbNfmqH3jZCOIq7w1Heoc51i%2BzkjWwk7V%2BP8aColYo3h0%2BykzFEyUik69WuQy&X-Amz-Signature=9da4035ddbd026156e0d1a2ceb3c5ffd8eb35c43656c270a6d8c4d3cca486b8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

