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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTXB4ILE%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T100053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAzs0ll1i1EMOqjqF8H06Ba0NYR%2Bhswp714EbexVNJzwAiBCuqfao88YOXKFTckss4n9uO14OCDm9ZAGJyLMUN3dAyr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMxcNVL7xD6K39mxbmKtwD%2BmCe%2FPVkXGyD4WXLjda1c8F8MFnkCk9rYuUQcAkO50DldmYIDYo6xSn6LG8zmrWcaeSUAgqyo1QF5NiUWkAMbikX5pBumNXOgF92b8yXWKOq3Ikd%2BkIhppk2S5JDErj0MsHDDvqmTc%2BQ0x4uOhDmwttbqaiHhA5MDDA5XsQp3%2BrzK1dwRHqNb%2Fvo%2FaypwIlDT%2FruI9AhAg4w%2Bxw%2Bpaz1KxbzoYmsy2b6nKVa9SJ6NaLUo8YX2FjJGGIHWOzojt3Onb%2BA0yclnG0ZWSHayo7Chv5pTsq5h9BfJGThAdwPVzuLxvW6h825hs%2BIgTTvZU%2BLM3DL5VsckV6luE4%2BFAYQXhHqAN%2FsSnausxIoSryKPPJQBVoaUmSogPTUAONvKUqbVbCFpZFDiyt%2Bl3G8eDdHikshbY2P2LZUdguecqSKDGse0Js%2B5jb5jO1ETBGBgK9qHE%2FYoEEtzWmVhsxK61BkEXsgXF6Lp66ZzFoypo3rK0cYrFLNbrPDkEYM8ipiv3bab9u5i63RJs6887RSMyCL3B6gG5In8cJXA3YbPDk3ZT3IkaEXgdjRLrNSTwakA1fjImKRqEAeEyA93ZEX7UIg3iniVNH8Zb7IwyRIFRGbleMFFPuPxTKM3T7CIhMwgYPhyAY6pgEGH%2FA9tjRxBmNXdOF%2BmrlpXWV%2B5aMaWLeSzyI6Ipc%2FOdw%2BwCfR%2B9gy%2B%2Ftc5Gql3D2fca9l%2FQiRxz5GHTurP8faGh2MtC6WpMMfevsUPn1NNoNeT5x5KQ6ZcLBojOsQMkm%2B%2BlJETQCiMyyCOLRg7cM6Rc2aZLLdvAVQm0l%2BtL1211BS6YMcLuGhwbSnIJ2MfX670uuzGzU4zhVPdh0QKS3bIfQE4yZ0&X-Amz-Signature=5105002c1dd0403ef5a007e5f64bcaadcc224c9f9956095f35f10425476a949a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

