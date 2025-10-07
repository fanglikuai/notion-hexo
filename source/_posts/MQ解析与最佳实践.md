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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JNY2CNN%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWsFCUQGzAHguwgdTqWGtf%2F6sPeggUENhOCRfWeDhRmwIhAOvP35mY5XfybPP2j%2BNd4VRGYwhnatj%2FmeOtZ%2FxxzOQQKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyGoUVOeDkQkrd16fwq3AMC3sd7n6ynJ02Jy11c0HZrTWmBQkLvqxnGFpIXJNz2wZF8FTWJ%2FTn00FnmUx%2FsvIjqXI6%2Fr4SSAVOcomo3GEaIVr8srkorWnG%2BM0Bh3CLBu8SGilwrsCG%2Bd8rezAInaF9u9E1MQRrk6nekw7AQt6d8FtkKyo2RJ1DLZwR%2FnZXiWwd87ExiYd1TFxX17u1ItHHfCE3h3gQqgCU4cplxbHDfpiex9GMKHY36%2BpYe3jiXldi1aOtLaV4TV7Rhfm6YRFnz82db0TdoSKWQSPLBphpn%2BAOARHHvL0PRzGlMCEAZZg22ln%2BvM9HgEkH%2FkvxEm6%2F3ZpH8bABcFuYCiyYu4HvHWHYuUXcfehre8Jze0DwanfUvX7yOMJ1yq0nNsVGdA%2BJirmMosiShwLGUxJWYaIZp%2BLbSsGNzJRRzkJnxWl4KDXHJpmbldP5YZ%2BjEP5mZwioyL%2BU8bizSPEBUBaofdQZoMGpaveZ3fzJNnZzG%2FwPjqeA464q5pWgMHCrc8n1O6P8x6MIHcvkKCeD%2FZ6GHmHgRvQMRL0P43gQfGsKwJw3quKSE0CF%2Fi5qCmv5ye3rYGlEXqamAQg%2BH5lLwe1Ic9coU2NlrW2qh%2BCJ5V%2BWyGfQ41BJB2c3qoMZbr8yjXjC79ZDHBjqkAX0nNrM6ufb3bc6BOII%2BMwCx%2BO5AQ1jivdh8Jas8GvW%2BfWHtmLcfjLjJG4WeSY5DGkkVOre8XD3gPG6ufdu59n886CzfglTaZ4nKO%2BcFrZvfhhe%2Bm5HnOXMBYMXUBiWirV6fvJ55688EvISw9qGAMtccbvpm2khcmviEt%2B6J3JEMq0l6PWs4iUCZbo6ee7qI4DSRWEus%2FlI1lyKTNKqhuYNj2dMX&X-Amz-Signature=9c213c80d1d36fdd3d9a270f1fd1d0d782603c976593b724f66c8671fc68eb89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

