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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QT3LWVBR%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7PYnf28qqrdAisqC30qQ8gdHxMx3m1vevZKT%2BDAXf6wIhAJfmhKoy3IWG0w02g3rIlZO1kk78Hryr6Lc3pp0P7e44KogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyMtZXREh99Toi0NHkq3APSjNOyf6AYtph%2BAE6t%2F%2F%2B2i3AH4zfuOqrLVbN%2BoYvvS8CKmIKZzVshUfADZhwXxV8b%2B0W2SIeAMlaQW9rnVMGbZ6%2Bid8SX81bYSYOSbEsnH6ce6x9h6RvYwZSn3W4DsC3B1JeZIqaqbZ%2B6fYuu5%2B6Imz%2BIQRmvXe9R4cPPfVobnlueakA8kXKC4MTz9IZLpWZDHe%2B%2Bgu%2F4GQMiUMIq6GsIOwb1bKW9q9NQUSBu3fEj1fxcDIkVLi%2FPHEQM4tl5kAXWJhyejI3w%2B7rc3wDbmQLkytPrPJ%2BTijPRK1urGqi5VTGP1K9uFFIdMk%2BKtbsK0Z2nFqBmfY8KkOn5GuQ7Y%2BHGX2ozlo2e%2Fy2ABMa7MgnCwZ3byTJ07UJy%2FbnrIzzUoFcLEv09KdV%2FmT12uTREOQUOkLwjKTF8AahYW8gi64rb4H0T0pAYKKGOMlq4PsshI4xxolrqU6YAWxgcM5DRh8eXMUWrmPIV3BDKp%2BgNpyRm3KH5bLpMWUPDYeLIACk5MihY0trJsB0BgpfhapcfHARUtfXWnUzMZwhJAtYWJ0lOPzXTNX7JcVLrjMMiDM81oFviJC3DbBjftJCKUSO55ewc8xZ8ZBcuFVc5gVqcpBKnRXgN9ysnnmhqGy5QZzCUwerIBjqkAf98KZkR4fLpJEL4P4M68PKnz9wT4qiDd7vdylQFspC4S8FT%2F5zX5JhN5YJ7y%2FV4CwFXZKida0ZCT3JDT4CCL01Fts%2BdFUgVM2RVKVmTIHUJEpQCXNBmF8Lrgk34HeZpII%2Fz8LmAQpkZQKmfjWKbwixYITo%2BftVEw7LBtOHnRI0QfcEZ%2FN4lvuCnZKOQ2Naw0OihmAF5I08nWTodJ8aYE0MgxHoO&X-Amz-Signature=73632f97940074b34021f7b904a22cce0cf0f8d7bc9bfbf0443ff0cbd20a71a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

