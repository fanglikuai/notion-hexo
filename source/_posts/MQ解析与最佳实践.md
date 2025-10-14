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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V74R6XU2%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T210047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCvn6GiX4QA4QfCZTSSZc6Dq7Ih8WtsCcpzbYtDZd3ADwIhAMSgSeqXL2CPQ1538OWszclZ28AL43uucko%2FWXtEAlCVKv8DCGYQABoMNjM3NDIzMTgzODA1IgzT1wtEAS6PIDH5x7wq3AMIMHgr00Mi5COxhvYyfYAr7qZvbEg6Ki2KdXsiIFe56eL07WZ7ZKEzOnYSn9hJN1gM%2BBdertIXuFso4RZ7ziWPo20NI1bnJp0et7J8cGLdiGVJOd1YtLe2D0xuD8OZFnpZEGfroT9aanoeZAUbGMeBQlCyl3GhzZkQ1BBOCZtdCZSBT2JDptxti4Iicozy3IGavEGe4YMsItaxiXD1bDSZImBUPxOgWUuArQpDu%2BDFBbOYovd5QDytwddKOkHYGOrObBSCrqj%2BXDJC9jUsnw4BCO5%2BcCDF5BZQtGl4yUaEd9k1hTTjsm%2BSSZ4vXxs4vIW8JISqZoDaCAUM6dtIEIFHX99qu%2Ftp8SS%2FwOdCgDr7xJhTAVfO6fAnwxoLcb%2BFBK7aN0%2FW5v1WImGUbd4cbAQWdbeOrCNaga8ycZEGz0sbZpMRA0wHx8YfK%2FGVqsdTo6f629lu7fH47q3rufYJPTKT6F5qbfSwItS%2FOS5QH%2FsJFxkZMcVcTYgFoHMFnOGftOdJrKrZzdRM%2FjKsp5F4gMqq4ujkhS1ZW45HY4TlIko6IKIu2EYR%2FWY%2BI02gH0D1M9TE3tXr6wCVKAxumOQjVOXB%2FteKc7y2Mr3jpFOdPMORa5stbQWb%2B2WCN%2BK0rTDW47rHBjqkAdFJ6sdbtumu46zn4u%2BYK3pNfLzhXWb37yKHSRFL%2BH%2Fh4RDqP43VChCBdfdQwy%2BiO1kgrlherA%2FRusiuznpHWFnGsPRnO4StXy4cYlI2CgAV%2BWiB0U6QEcAX6HxC5dpFtsmfAtj0INXXHKOaOmpmAMFYFXY6kyA%2B3jeCR%2F99NAmjnZ0dkq%2FQ8eKYxnyZYuSUuFcdDgVYXoH%2By81eQLptQR6R7jjB&X-Amz-Signature=388586e5248846f6249bef20d75a72e75e110bb25fae989d7efee01e4deac9cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

