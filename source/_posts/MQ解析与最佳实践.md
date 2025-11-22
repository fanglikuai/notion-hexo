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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VQ7EV2IN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJGMEQCIAh%2Fmxhk2uit5ykJd44CHTPZ93XCup0KH1zD0Gf2RxePAiBbQqrl0VhNfSz5q3pooVljfmcFpx2ebBtlvvveCGkclir%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMf8KPq5BT86H3eF%2FKKtwDglOPJM1QjZuvwgVthhfM4iWCdrAs7xgGtsV%2BzMMHdWPI8thNeg3dQ88gX7Sd2EaMkSskRtXfbYwpEp2D80w2JZSjGkN7VY3JDAFd%2BlRnuOmJ0hTu9odBQkp2XLfJIH4VuhJ6YOTyXxg%2B3HpBT4LPAb279I2vTKx3u5HTBY7BdOCCugzb3osuzlBv946ECXVBMyqDQfuzs5%2F3c5FxDbvq1fsel6adBrBsjKN99BmSFQRWa358O%2BmideB1Fa6XE2lmJnY%2B8uCM0rBtTLYByXEL3NI6NFR05p2Qeo1whcWZ%2FhaKI5x91%2FUpYmEUzJyx9LzRJ1UhP45MSImgMezXKViPhhlIyenvilX7g8MnDuEFT3P1Hpwx49LHUWnSCjNMX26qBlwnCx9ydlBhBBT%2FTuMP0q4YBs%2BEG0T9W1cGPiUG5wdHJLxi82WvqvzND0jFu5wrqHbj4zOz5L%2BgXsflOnvAUBFCtB%2BVrXnmqEHHB6iYt2Gqm%2BQn63kCGNWN1fPQZ2e%2F9PF6BixzrfrJsKgzB%2FJvS%2FY%2BkAqdmoKgrkDlUHH8RbzINE7rwiUwyjnfPzNZ2CyL7u%2BiCCLXZHFvWZd8ovElrxIubFy%2Fioafh51C1itZ%2FyxNynjkrB4SAdbeLo0wzo2EyQY6pgGb3uMxYf6K7b1OL5foOMKlhddlkPEzM6epnQinY1gtuD8gn3mx1%2Fuz6Y00aw0UnPPArhnGQPv1vApeDAQTSZE2shK3EdIXzYUrqab5WXSv3PC1hf4LanAeCtSdMKGf0gpV%2BESLUA6e%2FVPTOYr78ih8wUqLKGza8C8yJSwjQqgaj6075nu7aTxPI7DWmxPNulkBsPkKpHwUsJixu%2FlxxQUYdFGpqSEO&X-Amz-Signature=e40841dafd1ce214b7844771fa8534a893cb05a28eba45b1885f92500aff3d72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

