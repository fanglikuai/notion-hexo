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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OMZE6VG%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDPQ6AEtmof3z%2BCQWS7DlCr05fS3fqB%2B0AzB0pGPzlkLgIhALNOTiPo1ayvngwVilWRRwnd0Vh3JLVBUENa6TQD9vfqKv8DCEQQABoMNjM3NDIzMTgzODA1Igx23GSK0JK4VWs48sMq3AMI7rDDdUe7LXZnuRNVb9HL9e49GXLwgbhToXZqaSqGIWHyjJt45DD25lfXuxyCy7EjT25KxWkkuh0BiyAev3QCh%2B0bq%2FMhqJQwQZiux52mX6k9Ytl99vOzfOwolKtm2DI4oInx9992MR8orYTBVsRaKxmwBcgqtpIBSDi0KncWfNfe9As38qXm3n8ATUWV7nMIBTeCccYi6uSODNfotHLzxLhzKbhoE5l2XzEPtU8tQXppY4fRntCyl9jJw76c3Cp3O%2FQVQ5HfjCb8YsPy1yS72VwpyAixO41Z3Q1NxtH%2BraeW5nBlLFbmS7vJc28tUI8F72x2K63oZ5aI34P9dRcSAc6l%2FRAWs%2BdxxziLWw2JjuVCYWONE3GV2eu8XGis6aRe4jRvuaFaz7l1TrQLPJf4aXF%2FdVNK%2F0BE7uZPXaYEjelEJ06CkBM8oC5kMQJN%2BqEkakniNoiVL3VP%2BeicXJwMqqNBy7ZRFdVqOj2IJ6rkwk9cuQkNEjrbwh5G5RM1sjJzPzFai9Ll76zeegcL%2BfuBwqKRKR%2FEZPFP7e5t4PLJZe0nQIELiEFOXl2%2F8wQLvqw11qVI%2FSDldKUBX%2FgNE6ynzkLihjsL2yQDgFH4NNj5opJmoqPwyiV7epogoDD1l%2BjHBjqkARyewTPkSiSf12QxqaV%2BuEBvpBR%2F7GGt824VAxYJOWtsvVhENIPhuqd9u9uLSTt7iy3%2BzJ4jO2DXVR%2BJAbNT%2FDGLPF2c4Q5q91OKZds76CXFBF%2FYGiXW0rhIS3UvVBJik2xmFIvwPCRa1XmL5uEPvGazihLf9wVAGjqorPehXimThDpTn5X%2BkFI9O0HTdJWCs5RJuS%2Fng08hco0bfCokE7i5Lpqu&X-Amz-Signature=add3ea0d5e72ec66cb3c7f2054729d43a17509cad8e7299e98fa787be0525d82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

