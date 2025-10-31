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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UJBX6VS%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQCis3Brk4IshlAQqwndtIvjKBWpWeHOnvIWehiqCyMJNgIhAKcTkZQnajxACRfkjx00ryi1%2F29VJiKDNKYnHv9gxancKv8DCB8QABoMNjM3NDIzMTgzODA1IgyTeGSy7f6mgnZbIMAq3AOhIYpgkHR78%2BaEBBGi%2FD2jjP2GBGIGLq3ssGUwhV6WvlA6n%2B6wFSreilwg6n9TGa6vTKK5Q05p17dIhzmBsXgy%2BlUy5ekyL7VO00dDsptfEMXjiQUsPdpiN4G5zDS6TvHOZ6Lu1i5od68P8VGpf9NJ4tkRVkr8LHtsTxRN%2F3%2BD1NxADY5N8AeWGZhw9PRDhts9%2B4TC8Pe4h6uDclo%2FTn%2BbcZ5weyQrw%2B9dKPTgGPSyRI0Fkp9N%2FGekB0atpdrOVNJoOvmRvejo5S03ePjRMh5YUzh%2B%2FE%2Bh9vbaQlL1WlMmWQMhm%2F3BKycyRw0miOHvgFxts9U8jU1j5wxqVocqdlKu%2FUAzopVTnG4aDiiS3Bjx%2Bo9wdcQsWsmQ1eRN4zIBAHIYsCoAMWZZd%2FKZ4PDK4G7kU3Cevzyi9jd4R0m67x91gDya%2FmsgjbcuvwDNJGz85zpKnQ21cy3PpiOe7QASz3bDXgt1eTLd1%2BB1hbglS8Le%2BjZWU3UiwqxxWmr2X3yZpWJcKCQjpjHdJsf%2FWcZdXx%2FPPoLnDLylnI7wWIXAy20NZ%2BV91h2jQgkOb3jnJNZNhoM8ds3bvpP9z4yqkagblNOSKmZxXPvEgcZgrKqEGmEqvR8Ci35bcGaF7R4uUTDK45TIBjqkAWI%2FfEv92w210RgkThP47KFrKxHfxXzQD0uAhGswPnHjnqoKwY1t2wWuaRcJlwucoZSSR%2FwCBn2w3jAkGr%2F%2F6ZYJqqDwpW0BRx10t%2FNZDKVc2wfJFULxcf8l6MrH3h3Kqu3xCeDXdDKbzZzx%2B5Bwabu0Je1ESrJx%2B5OAMjj7YArdn3W4YPH8XQ%2BRexwae1%2F7ZrgZJmRQGAZ0zPSGMxDMuw6AjY5F&X-Amz-Signature=d722e105466bf33c90d014c25b2715e10cdbc08b241ce1d492b4e5f76d9c180d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

